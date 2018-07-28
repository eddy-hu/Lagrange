---
layout: post
title: "Hibernate:HQL, Criteria and Sql"
author: "Eddy Hu"
categories: journal
tags: [hibernate,java]
image: database.jpg
---

#### HQL Queries

Hibernate Query Language (HQL) is an object-oriented query language, similar to SQL, but instead of operating on tables and columns, HQL works with persistent objects and their properties. HQL queries are translated by Hibernate into conventional SQL queries, which in turns perform action on database.  
Syntax: [select/update/delete……] from Entity [where……] [group by……] [having……] [order by……]  

##### Without Spring framework  

5 steps to use HQL:  

-   Get Session  
    
-   Write Query Statement  
    
-   Instantiate a Query object  
    
-   Execute Query  
    
-   Get result  
    

Example:  

```
sessionFactory = new Configuration().configure().buildSessionFactory();
session = sessionFactory.openSession();
String hql = “from Customer”;
Query query = session.createQuery(hql);
List<Customer> list = query.list();

```

##### With Spring framework  

```
String queryString = "select form entity ....";

List list=getHibernateTemplate().find(queryString);

```

#### Criteria Queries

The Hibernate Session interface provides createCriteria() method, which can be used to create a Criteria object that returns instances of the persistence object’s class when your application executes a criteria query.  

##### Without Spring framework  

```
  SessionFactory sessionFactory = new Configuration().configure()
          .buildSessionFactory();
  Session session = sessionFactory.openSession();
  Criteria criteria = session.createCriteria(User.class);
  List result = criteria.list();
  Iterator it = result.iterator();

```

##### With Spring framework  

```
import org.hibernate.criterion.DetachedCriteria;

DetachedCriteria criteria=DetachedCriteria.forClass(ObjectEntity.class);

criteria.add(Restrictions.eq("propertyName", propertyValue));

List result=getHibernateTemplate().findByCriteria(criteria);

```

#### Native SQL Queries (With Spring)  

##### Return View Object  

```
List list = getHibernateTemplate().executeFind(new HibernateCallback() {

  public Object doInHibernate(Session session) throws HibernateException, SQLException {

 StringBuffer hqlBuffer = new StringBuffer("");
 hqlBuffer.append("select column_Name  from ...");
 SQLQuery sqlQuery = session.createSQLQuery(hqlBuffer.toString());
 sqlQuery.addScalar("propertyName",Hibernate.STRING);
 sqlQuery.setResultTransformer(Transformers.aliasToBean(ObjectVO.class));    
  List list = sqlQuery.list();
  return list;
}
 });

```

##### Return List

```
  final String queryString = "";
  List resultList=getHibernateTemplate().executeFind(new HibernateCallback() {
  public List doInHibernate(Session session) throws HibernateException, SQLException {
  SQLQuery sqlQuery = session.createSQLQuery(queryString);
  List list=sqlQuery.executeUpdate();
  return list;
   }
 });

```

##### Return Null

```
final String queryString = "";
getHibernateTemplate().executeFind(new HibernateCallback() {
public Object doInHibernate(Session session) throws HibernateException, SQLException {
SQLQuery sqlQuery = session.createSQLQuery(queryString);
sqlQuery.executeUpdate();
return null;

 } });
```
