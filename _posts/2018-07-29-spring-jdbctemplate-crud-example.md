---
layout: post
title: "Spring JdbcTemplate CRUD example"
author: "Eddy Hu"
categories: journal
tags: [spring,java]
image: spring-jdbc-example.jpg
---
#### UserDao.java

    import java.util.List;
    import bean.User;
    public interface UserDao {
    
    	void add(User user);
    	void delete(Integer id);
    	void update(User user);
    	User getById(Integer id);
    	int getTotalCount();
    	List<User> getAll();
    }


#### UserDaoImp.java

   

    import java.sql.ResultSet;
    import java.sql.SQLException;
    import java.util.List;
    
    import org.springframework.jdbc.core.JdbcTemplate;
    import org.springframework.jdbc.core.RowMapper;
    import org.springframework.jdbc.core.support.JdbcDaoSupport;
    import bean.User;
    public class UserDaoImp extends JdbcDaoSupport implements UserDao {
    	@Override
    	public void add(User user) {
    		String sql = "insert into users values(null,?) ";
    		super.getJdbcTemplate().update(sql, user.getName());
    	}
    	@Override
    	public void delete(Integer id) {
    		String sql = "delete from users  where id = ? ";
    		super.getJdbcTemplate().update(sql,id);
    	}
    	@Override
    	public void update(User user) {
    		String sql = "update  users  set name = ? where id=? ";
    		super.getJdbcTemplate().update(sql, user.getName(),user.getId());
    	}
    	@Override
    	public User getById(Integer id) {
    		String sql = "select * from users  where id = ? ";
    		return super.getJdbcTemplate().queryForObject(sql,new RowMapper<User>(){
    			@Override
    			public User mapRow(ResultSet rs, int arg1) throws SQLException {
    				User u = new User();
    				u.setId(rs.getInt("id"));
    				u.setName(rs.getString("name"));
    				return u;
    			}}, id);
    		
    	}
    	@Override
    	public int getTotalCount() {
    		String sql = "select count(*) from users   ";
    		Integer count = super.getJdbcTemplate().queryForObject(sql, Integer.class);
    		return count;
    	}
    
    	@Override
    	public List<User> getAll() {
    		String sql = "select * from users   ";
    		List<User> list = super.getJdbcTemplate().query(sql, new RowMapper<User>(){
    			@Override
    			public User mapRow(ResultSet rs, int arg1) throws SQLException {
    				User u = new User();
    				u.setId(rs.getInt("id"));
    				u.setName(rs.getString("name"));
    				return u;
    			}});
    		return list;
    	}
    
    
    }

