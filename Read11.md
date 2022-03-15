# Spring MVC 

---

## Spring model attributes 

Spring MVC calls the pieces of data that can be accessed during the execution of views model attributes. The equivalent term in Thymeleaf language is context variables.

There are several ways of adding model attributes to a view in Spring MVC. Below you will find some common cases:

Add attribute to Model via its addAttribute method:

    @RequestMapping(value = "message", method = RequestMethod.GET)
    public String messages(Model model) {
    model.addAttribute("messages", messageRepository.findAll());
    return "message/list";
    }

Return ModelAndView with model attributes included:

    @RequestMapping(value = "message", method = RequestMethod.GET)
        public ModelAndView messages() {
            ModelAndView mav = new ModelAndView("message/list");
            mav.addObject("messages", messageRepository.findAll());
            return mav;
        }


## Request parameters

Request parameters can be easily accessed in Thymeleaf views. Request parameters are passed from the client to server like:

    https://example.com/query?q=Thymeleaf+Is+Great!


## Session attributes

In the below example we add mySessionAttribute to session:

    @RequestMapping({"/"})
        String index(HttpSession session) {
            session.setAttribute("mySessionAttribute", "someValue");
            return "index";
        }

## ServletContext attributes

The ServletContext attributes are shared between requests and sessions. In order to access ServletContext attributes in Thymeleaf you can use the #servletContext. prefix:

        <table>
                <tr>
                    <td>My context attribute</td>
                    <!-- Retrieves the ServletContext attribute 'myContextAttribute' -->
                    <td th:text="${#servletContext.getAttribute('myContextAttribute')}">42</td>
                </tr>
                <tr th:each="attr : ${#servletContext.getAttributeNames()}">
                    <td th:text="${attr}">javax.servlet.context.tempdir</td>
                    <td th:text="${#servletContext.getAttribute(attr)}">/tmp</td>
                </tr>
            </table>


## Spring beans

Thymeleaf allows accessing beans registered at the Spring Application Context with the @beanName syntax, for example:

    <div th:text="${@urlService.getApplicationUrl()}">...</div> 