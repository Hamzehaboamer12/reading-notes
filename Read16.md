# Spring Authentication

---

## Spring Security Architecture


This guide is a primer for Spring Security, offering insight into the design and basic building blocks of the framework. We cover only the very basics of application security. However, in doing so, we can clear up some of the confusion experienced by developers who use Spring Security. To do this, we take a look at the way security is applied in web applications by using filters and, more generally, by using method annotations. Use this guide when you need a high-level understanding of how a secure application works, how it can be customized, or if you need to learn how to think about application security.

This guide is not intended as a manual or recipe for solving more than the most basic problems (there are other sources for those), but it could be useful for beginners and experts alike. Spring Boot is also often referenced, because it provides some default behavior for a secure application, and it can be useful to understand how that fits in with the overall architecture.


## Authentication and Access Control


Application security boils down to two more or less independent problems: authentication (who are you?) and authorization (what are you allowed to do?). Sometimes people say “access control” instead of "authorization", which can get confusing, but it can be helpful to think of it that way because “authorization” is overloaded in other places. Spring Security has an architecture that is designed to separate authentication from authorization and has strategies and extension points for both.


    public interface AuthenticationManager {

    Authentication authenticate(Authentication authentication)
    throws AuthenticationException;
    }

An AuthenticationManager can do one of 3 things in its authenticate() method:

Return an Authentication (normally with authenticated=true) if it can verify that the input represents a valid principal.

Throw an AuthenticationException if it believes that the input represents an invalid principal.

Return null if it cannot decide.

AuthenticationException is a runtime exception. It is usually handled by an application in a generic way, depending on the style or purpose of the application. In other words, user code is not normally expected to catch and handle it. For example, a web UI might render a page that says that the authentication failed, and a backend HTTP service might send a 401 response, with or without a WWW-Authenticate header depending on the context.



     public interface AuthenticationProvider {
          Authentication authenticate(Authentication authentication)
                throws AuthenticationException;
    
        boolean supports(Class<?> authentication);
        }


The Class<?> argument in the supports() method is really Class<? extends Authentication> (it is only ever asked if it supports something that is passed into the authenticate() method). A ProviderManager can support multiple different authentication mechanisms in the same application by delegating to a chain of AuthenticationProviders. If a ProviderManager does not recognize a particular Authentication instance type, it is skipped.

A ProviderManager has an optional parent, which it can consult if all providers return null. If the parent is not available, a null Authentication results in an AuthenticationException.

Sometimes, an application has logical groups of protected resources (for example, all web resources that match a path pattern, such as /api/**), and each group can have its own dedicated AuthenticationManager. Often, each of those is a ProviderManager, and they share a parent. The parent is then a kind of “global” resource, acting as a fallback for all providers.



## Authorization or Access Control

Once authentication is successful, we can move on to authorization, and the core strategy here is AccessDecisionManager. There are three implementations provided by the framework and all three delegate to a chain of AccessDecisionVoter instances, a bit like the ProviderManager delegates to AuthenticationProviders.

An AccessDecisionVoter considers an Authentication (representing a principal) and a secure Object, which has been decorated with ConfigAttributes:

    boolean supports(ConfigAttribute attribute);
    
    boolean supports(Class<?> clazz);

int vote(Authentication authentication, S object,
Collection<ConfigAttribute> attributes);
The Object is completely generic in the signatures of the AccessDecisionManager and AccessDecisionVoter. It represents anything that a user might want to access (a web resource or a method in a Java class are the two most common cases). The ConfigAttributes are also fairly generic, representing a decoration of the secure Object with some metadata that determines the level of permission required to access it. ConfigAttribute is an interface. It has only one method (which is quite generic and returns a String), so these strings encode in some way the intention of the owner of the resource, expressing rules about who is allowed to access it. A typical ConfigAttribute is the name of a user role (like ROLE_ADMIN or ROLE_AUDIT), and they often have special formats (like the ROLE_ prefix) or represent expressions that need to be evaluated.


Web Security

Spring Security in the web tier (for UIs and HTTP back ends) is based on Servlet Filters, so it is helpful to first look at the role of Filters generally. The following picture shows the typical layering of the handlers for a single HTTP request.

Filter chain delegating to a Servlet
The client sends a request to the application, and the container decides which filters and which servlet apply to it based on the path of the request URI. At most, one servlet can handle a single request, but filters form a chain, so they are ordered. In fact, a filter can veto the rest of the chain if it wants to handle the request itself. A filter can also modify the request or the response used in the downstream filters and servlet. The order of the filter chain is very important, and Spring Boot manages it through two mechanisms: @Beans of type Filter can have an @Order or implement Ordered, and they can be part of a FilterRegistrationBean that itself has an order as part of its API. Some off-the-shelf filters define their own constants to help signal what order they like to be in relative to each other (for example, the SessionRepositoryFilter from Spring Session has a DEFAULT_ORDER of Integer.MIN_VALUE + 50, which tells us it likes to be early in the chain, but it does not rule out other filters coming before it).


Creating and Customizing Filter Chains
The default fallback filter chain in a Spring Boot application (the one with the /** request matcher) has a predefined order of SecurityProperties.BASIC_AUTH_ORDER. You can switch it off completely by setting security.basic.enabled=false, or you can use it as a fallback and define other rules with a lower order. To do the latter, add a @Bean of type WebSecurityConfigurerAdapter (or WebSecurityConfigurer) and decorate the class with @Order, as follows:

    @Configuration
    @Order(SecurityProperties.BASIC_AUTH_ORDER - 10)
    public class ApplicationConfigurerAdapter extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
    http.antMatcher("/match1/**")
    ...;
    }
    }

Request Matching for Dispatch and Authorization
A security filter chain (or, equivalently, a WebSecurityConfigurerAdapter) has a request matcher that is used to decide whether to apply it to an HTTP request. Once the decision is made to apply a particular filter chain, no others are applied. However, within a filter chain, you can have more fine-grained control of authorization by setting additional matchers in the HttpSecurity configurer, as follows:

    @Configuration
    @Order(SecurityProperties.BASIC_AUTH_ORDER - 10)
    public class ApplicationConfigurerAdapter extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
    http.antMatcher("/match1/**")
    .authorizeRequests()
    .antMatchers("/match1/user").hasRole("USER")
    .antMatchers("/match1/spam").hasRole("SPAM")
    .anyRequest().isAuthenticated();
    }
    }


Method Security

As well as support for securing web applications, Spring Security offers support for applying access rules to Java method executions. For Spring Security, this is just a different type of “protected resource”. For users, it means the access rules are declared using the same format of ConfigAttribute strings (for example, roles or expressions) but in a different place in your code. The first step is to enable method security — for example, in the top level configuration for our application:

    @SpringBootApplication
    @EnableGlobalMethodSecurity(securedEnabled = true)
    public class SampleSecureApplication {
    }