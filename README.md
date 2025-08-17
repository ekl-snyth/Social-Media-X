CREATE DATABASE IF NOT EXISTS Social_Media;
USE Social_Media;

DROP TABLE IF EXISTS Likes;
DROP TABLE IF EXISTS Followers;
DROP TABLE IF EXISTS Posts;
DROP TABLE IF EXISTS Users;

CREATE TABLE Users(
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL,
    password VARCHAR(30) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Posts(
    post_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    content VARCHAR(500) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY(user_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Followers(
    follower_id INT,
    following_id INT,
    followed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY(follower_id, following_id),
    FOREIGN KEY(follower_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY(following_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Likes(
    user_id INT,
    post_id INT,
    liked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY(user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY(post_id) REFERENCES Posts(post_id) ON DELETE CASCADE
);

INSERT INTO Users(username, email, password) VALUES
('eklavya43', 'eklavya@gmail.com', 'rajput001'),
('cyberpunk001', 'punk@gmail.com', 'security4'),
('ashwin_99', 'ashwin@example.com', 'darkchoco'),
('anime_fan', 'animefan@example.com', 'otaku123'),
('coderX', 'coderx@example.com', 'code4life');

INSERT INTO Posts(user_id, content) VALUES
(1, 'Hello world! This is my first post.'),
(2, 'I love dark chocolate!'),
(3, 'Good morning everyone 🌸'),
(4, 'One Piece > All other anime.'),
(5, 'Debugging feels like detective work 🔎');

INSERT INTO Followers(follower_id, following_id) VALUES
(1, 2), (2, 1), (3, 1), (4, 5), (5, 3);

INSERT INTO Likes(user_id, post_id) VALUES
(1, 2), (2, 1), (3, 1), (4, 3), (5, 4);

SELECT * FROM Users;
SELECT * FROM Posts;

SELECT Users.username, Posts.content, Posts.created_at
FROM Posts
JOIN Users ON Posts.user_id = Users.user_id
WHERE Users.username = 'eklavya43'
ORDER BY Posts.created_at DESC
LIMIT 0, 1000;

SELECT u.username AS follower
FROM Followers f
JOIN Users u ON f.follower_id = u.user_id
WHERE f.following_id = 1;

SELECT u.username AS liked_by
FROM Likes l
JOIN Users u ON l.user_id = u.user_id
WHERE l.post_id = 1;

UPDATE Users
SET email = 'eklavya_new@example.com'
WHERE user_id = 1;

UPDATE Posts
SET content = 'Hello world! Updated my first post.'
WHERE post_id = 1;

DELETE FROM Likes
WHERE user_id = 1 AND post_id = 2;

DELETE FROM Posts
WHERE post_id = 2;

DELETE FROM Users
WHERE user_id = 2;
