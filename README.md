# Instagram-SQL

 I successfully completed a comprehensive SQL course on Udemy, mastering various SQL functions. As a culmination of my learning journey, I undertook a capstone project where I designed a database using simulated Instagram data. After creating the database, I was challenged with answering several questions as shown below.

Database Schema:

```sql
CREATE DATABASE ig_clone;
USE ig_clone;

CREATE TABLE users (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE photos (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    image_url VARCHAR(255) NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id)
);

CREATE TABLE comments (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    comment_text VARCHAR(255) NOT NULL,
    photo_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(user_id) REFERENCES users(id)
);

CREATE TABLE likes (
    user_id INTEGER NOT NULL,
    photo_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    PRIMARY KEY(user_id, photo_id)
);

CREATE TABLE follows (
    follower_id INTEGER NOT NULL,
    followee_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(follower_id) REFERENCES users(id),
    FOREIGN KEY(followee_id) REFERENCES users(id),
    PRIMARY KEY(follower_id, followee_id)
);

CREATE TABLE tags (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    tag_name VARCHAR(255) UNIQUE,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE photo_tags (
    photo_id INTEGER NOT NULL,
    tag_id INTEGER NOT NULL,
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(tag_id) REFERENCES tags(id),
    PRIMARY KEY(photo_id, tag_id)
);
```

Insert Data into Tables:

I did not insert all of the data due to the size.

![image](https://github.com/user-attachments/assets/8e3a914c-8842-4c36-a55a-65cfc336a44e)


Challenge 1: Find the 5 oldest users.
```sql
select * from users
order by created_at
limit 5;
```

![SS3](https://github.com/user-attachments/assets/67569c2e-e068-45f3-a0e8-666e81b47938)


Challenge 2: Whay day of the week do most users register on?
```sql
select 
	dayname(created_at) as 'day', 
	count(*) as total
from users
group by day
order by total
desc
limit 2;
```

![SS4](https://github.com/user-attachments/assets/1cf82767-a793-4918-95c1-4f7ae67e6ee6)


Challenge 3: Find the users who have never posted a photo.
```sql
select username, image_url from users
left join photos
	on users.id = photos.user_id
where image_url is null;
```

![SS5](https://github.com/user-attachments/assets/9a48b311-6da1-471e-96c5-e2abc8048330)


Challenge 4: Which user has the most likes on a single photo?
```sql
select 
	username,
	photos.id, 
	photos.image_url, 
	count(*) as total
from photos
join likes
	on photos.id = likes.photo_id
join users
	on photos.user_id = users.id
group by photos.id
order by total
desc
limit 1;
```

![SS6](https://github.com/user-attachments/assets/4258b4c4-b21c-43d8-85a2-d6c77aca807e)


Challenge 5: How many times does the average user post?
```sql
select
	round((select count(*) from photos) / (select count(*) from users),2) as avg_user_post;
```

 ![SS7](https://github.com/user-attachments/assets/d2bb4408-e53e-4d13-9ab5-10be31d15b74)


 Chalenge 6: What are the top 5 most commonly used hashtags?
 ```sql
 select 
	tag_name,
    count(*) as total
from tags
join photo_tags
	on photo_tags.tag_id = tags.id
group by tags.id
order by total
desc
limit 7;
```

![SS9](https://github.com/user-attachments/assets/444018ab-4da9-4ce0-80f9-7cfe7768f886)










