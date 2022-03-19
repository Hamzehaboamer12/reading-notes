# CrudRepository, JpaRepository, and PagingAndSortingRepository in Spring Data 

---

* Spring Data Repositories
 
Let's start with the JpaRepository – which extends PagingAndSortingRepository and, in turn, the CrudRepository.

Each of these defines its own functionality:

1- CrudRepository provides CRUD functions

2- PagingAndSortingRepository provides methods to do pagination and sort records

3- JpaRepository provides JPA related methods such as flushing the persistence context and delete records in a batch


* CrudRepository

  * Let's now have a look at the code for the CrudRepository interface:
  

      public interface CrudRepository<T, ID extends Serializable>
      extends Repository<T, ID> {
    
          <S extends T> S save(S entity);
    
          T findOne(ID primaryKey);
    
          Iterable<T> findAll();
    
          Long count();
    
          void delete(T entity);
    
          boolean exists(ID primaryKey);
      }

CRUD functionality:

* save(…) – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
* findOne(…) – get a single entity based on passed primary key value
* findAll() – get an Iterable of all available entities in database
* count() – return the count of total entities in a table
* delete(…) – delete an entity based on the passed object
* exists(…) – verify if an entity exists based on the passed primary key value


* PagingAndSortingRepository

    
    AD
    public interface PagingAndSortingRepository<T, ID extends Serializable>
    extends CrudRepository<T, ID> {
    
        Iterable<T> findAll(Sort sort);
    
        Page<T> findAll(Pageable pageable);
    }



* JpaRepository


    public interface JpaRepository<T, ID extends Serializable> extends
    PagingAndSortingRepository<T, ID> {

    List<T> findAll();

    List<T> findAll(Sort sort);

    List<T> save(Iterable<? extends T> entities);

    void flush();

    T saveAndFlush(T entity);

    void deleteInBatch(Iterable<T> entities);
    }


-  let's look at each of these methods in brief:

* findAll() – get a List of all available entities in database
* findAll(…) – get a List of all available entities and sort them using the provided condition
* save(…) – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
* flush() – flush all pending task to the database
* saveAndFlush(…) – save the entity and flush changes immediately