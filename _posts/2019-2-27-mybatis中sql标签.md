---
layout: post
title: Mybatis中sql标签
description: Mybatis中sql标签
category: javaweb
---

# 一、if标签
### 使用 if 动态 sql 语句先进行判断，如果值为 null 或等于空字符串，我们就不进行此条件的判断，增加灵活性。
	<select id="findUserList" parameterType="user" resultType="user">
		select * from user  where 1=1
		<if test="id!=null and id!=''">
			and id=#{id}
		</if>
		<if test="username!=null and username!=''">
			and username like '%${username}%'
		</if>
	</select>

# 二、where标签
	// 简化 sql 语句中 where 条件判断，自动地处理 AND/OR 条件。
	<select id="getUserList_whereIf" resultMap="resultMap_User" parameterType="com.yiibai.pojo.User">  
    SELECT u.user_id,  
           u.username,  
           u.sex,  
           u.birthday 
      FROM User u
    <where>  
        <if test="username !=null ">  
            u.username LIKE CONCAT(CONCAT('%', #{username, jdbcType=VARCHAR}),'%')  
        </if>  
        <if test="sex != null and sex != '' ">  
            AND u.sex = #{sex, jdbcType=INTEGER}  
        </if>  
        <if test="birthday != null ">  
            AND u.birthday = #{birthday, jdbcType=DATE}  
        </if> 
    </where>    
	</select> 

# 三、 set标签
	// 主要功能和 where 标签元素其实是差不多的，主要是在包含的语句前输出一个 set，
    然后如果包含的语句是以逗号结束的话将会把该逗号忽略，如果 set 包含的内容为空的话则会出错。有了 set 元素就可以动态的更新那些修改了的字段。 
	<update id="dynamicSetTest" parameterType="Blog">
        update t_blog
        <set>
            <if test="title != null">
                title = #{title},
            </if>
            <if test="content != null">
                content = #{content},
            </if>
            <if test="owner != null">
                owner = #{owner}
            </if>
        </set>
        where id = #{id}
    </update>

# 四、choose,when,otherwise标签
	// 即基本数据结构：switch,case,default
	<select id="dynamicChooseTest" parameterType="Blog" resultType="Blog">
        select * from t_blog where 1 = 1 
        <choose>
            <when test="title != null">
                and title = #{title}
            </when>
            <when test="content != null">
                and content = #{content}
            </when>
            <otherwise>
                and owner = "owner1"
            </otherwise>
        </choose>
    </select>

# 五、trim标签
### 即可以当成where有可以当成set
	<trim prefix="WHERE" prefixoverride="AND | OR">
		<if test="id != null">AND id=#{id}</if>
		<if test="name != null and name.length()>0">AND name=#{name}</if>
		<if test="gender != null and gender.length()>0">AND gender=#{gender}</if>
	</trim>

	<trim prefix="SET" suffixOverrides=",">  
        <if test="username != null and username != '' ">  
            username = #{username},  
        </if>  
        <if test="sex != null and sex != '' ">  
            sex = #{sex},  
        </if>  
        <if test="birthday != null ">  
            birthday = #{birthday},  
        </if>  
    </trim> 

# 六、foreach标签
### foreach的主要用在构建in条件中，它可以在SQL语句中进行迭代一个集合。

### foreach元素的属性主要有 item，index，collection，open，separator，close。

- item表示集合中每一个元素进行迭代时的别名，
- index指 定一个名字，用于表示在迭代过程中，每次迭代到的位置，
- open表示该语句以什么开始，
- separator表示在每次进行迭代之间以什么符号作为分隔 符，
- close表示以什么结束。

### 在使用foreach的时候最关键的也是最容易出错的就是collection属性，该属性是必须指定的，但是在不同情况 下，该属性的值是不一样的，主要有一下3种情况：

1. 如果传入的是单参数且参数类型是一个List的时候，collection属性值为list
2. 如果传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array
3. 如果传入的参数是多个的时候，我们就需要把它们封装成一个Map了，当然单参数也可

### 以封装成map，实际上如果你在传入参数的时候，在breast里面也是会把它封装成一个Map的，map的key就是参数名，所以这个时候collection属性值就是传入的List或array对象在自己封装的map里面的key
	<select id="dynamicForeachTest"parameterType="java.util.List"resultType="Blog">
           select * from t_blog where id in
   			<foreach collection="list" index="index" item="item" open="("separator=","close=")">
                #{item}       
        	</foreach>    
    </select>

# 七、sql片段
###   Sql中可将重复的sql提取出来，使用时用include引用即可，最终达到sql重用的目的，如下：
	//建立sql片段
	<sql id="query_user_where">
		<if test="id!=null and id!=''">
			and id=#{id}
		</if>
		<if test="username!=null and username!=''">
			and username like '%${username}%'
		</if>
	</sql>

	//使用include引用sql片段
	<select id="findUserList" parameterType="user" resultType="user">
		select * from user
			<where>
				<include refid="query_user_where"/>
			</where>
	</select>

	//引用其它mapper.xml的sql片段
	<include refid="namespace.sql片段"/>