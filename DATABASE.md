## Скрипт для создания базы данных:
```sql
CREATE TABLE Cities (
	id serial PRIMARY KEY,
	name VARCHAR(255) NOT NULL
);

CREATE TABLE Educations (
	id serial PRIMARY KEY,
	user_id INT NOT NULL,
	name VARCHAR(300) NOT NULL,
	start_date TIMESTAMP NOT Null,
	end_date TIMESTAMP NULL,
	type_id INT NOT NULL,
	FOREIGN KEY (user_id) REFERENCES Users(id),
	FOREIGN KEY (type_id) REFERENCES EducationsTypes(id)
);

CREATE TABLE EducationsTypes (
	id serial PRIMARY KEY,
	name VARCHAR(255) NOT NULL
);

CREATE TABLE ExpertReviews (
	expert_user_id INT NOT NULL,
	user_id INT NOT NULL,
	rating SMALLINT NOT NULL,
	description VARCHAR(300) NOT NULL,
	PRIMARY KEY (expert_user_id, user_id),
	FOREIGN KEY (expert_user_id) REFERENCES Users(id),
	FOREIGN KEY (user_id) REFERENCES Users(id)
);

CREATE TABLE Interview (
	id serial PRIMARY KEY,
	start_date TIMESTAMP NULL,
	end_date TIMESTAMP NULL,
	feedback VARCHAR(1000) NULL,
	rating SMALLINT NULL,
	description VARCHAR(500) NOT NULL,
	postition_id INT NOT NULL,
	status_id INT NOT NULL,
	created_at TIMESTAMP NOT NULL DEFAULT NOW(),
	updated_at TIMESTAMP NOT NULL DEFAULT NOW(),
	user_id INT NOT NULL,
	level_id INT NOT NULL,
	price MONEY NOT NULL,
	FOREIGN KEY (postition_id) REFERENCES InterviewStatus(id),
	FOREIGN KEY (level_id) REFERENCES Levels(id),
	FOREIGN KEY (user_id) REFERENCES Users(id)
);

CREATE TABLE InterviewMessages (
	id serial PRIMARY KEY,
	user_id INT NOT NULL,
	interview_id INT NOT NULL,
	message VARCHAR(500) NOT NULL,
	created_at TIMESTAMP NOT NULL,
	FOREIGN KEY (user_id) REFERENCES Users(id),
	FOREIGN KEY (interview_id) REFERENCES Interview(id)
);

CREATE TABLE InterviewReviews (
	expert_user_id INT NOT NULL,
	interview_id INT NOT NULL,
	rating SMALLINT NOT NULL,
	description VARCHAR(300) NOT NULL,
	PRIMARY KEY (expert_user_id, interview_id),
	FOREIGN KEY (expert_user_id) REFERENCES Users(id),
	FOREIGN KEY (interview_id) REFERENCES Interview(id)
);

CREATE TABLE InterviewSkills (
	skill_id INT NOT NULL,
	interview_id INT NOT NULL,
	PRIMARY KEY (skill_id, interview_id),
	FOREIGN KEY (skill_id) REFERENCES Skills(id),
	FOREIGN KEY (interview_id) REFERENCES Interview(id)
);

CREATE TABLE InterviewStatus (
	id serial PRIMARY KEY,
	name VARCHAR(255) NOT NULL
);

CREATE TABLE Levels (
	id serial PRIMARY KEY,
	name VARCHAR(255) NOT NULL
);

CREATE TABLE Notifications (
	id serial PRIMARY KEY,
	user_id INT NOT NULL,
	message VARCHAR(300) NOT NULL,
	time_sending TIMESTAMP NOT NULL,
	readed BOOLEAN NOT NULL,
	FOREIGN KEY (user_id) REFERENCES Users(id)
);

CREATE TABLE Participants (
	interview_id INT NOT NULL,
	expert_user_id INT NOT NULL,
	creator BOOLEAN NOT NULL,
	PRIMARY KEY (interview_id, expert_user_id),
	FOREIGN KEY (interview_id) REFERENCES Interview(id),
	FOREIGN KEY (expert_user_id) REFERENCES Users(id)
);

CREATE TABLE Positions (
	id serial PRIMARY KEY,
	name VARCHAR(300) NOT NULL
);

CREATE TABLE Rates (
	id serial PRIMARY KEY,
	level VARCHAR(255) NOT NULL,
	description VARCHAR(255) NOT NULL,
	price MONEY NOT NULL
);

CREATE TABLE Roles (
	id serial PRIMARY KEY,
	name VARCHAR(255) NOT NULL
);

CREATE TABLE Skills (
	id serial PRIMARY KEY,
	name VARCHAR(255) NULL
);

CREATE TABLE UserMessages (
	id serial PRIMARY KEY,
	sender_id INT NOT NULL,
	receiver_id INT NOT NULL,
	message VARCHAR(500) NOT NULL,
	created_at TIMESTAMP NOT NULL,
	FOREIGN KEY (sender_id) REFERENCES Users(id),
	FOREIGN KEY (receiver_id) REFERENCES Users(id)
);

CREATE TABLE Users (
	id serial PRIMARY KEY,
	email VARCHAR(255) NULL,
	password VARCHAR(255) NOT NULL,
	firstname VARCHAR(255) NOT NULL,
	lastname VARCHAR(255) NOT NULL,
	patronymic VARCHAR(255) NULL,
	gender VARCHAR(15) NULL,
	phone VARCHAR(50) NULL,
	city_id INT NOT NULL,
	role_id INT NOT NULL,
	description VARCHAR(255) NULL,
	birth_date TIMESTAMP NOT NULL,
	avatar_path VARCHAR(255) NOT NULL,
	deleted BOOLEAN NOT NULL DEFAULT FALSE,
	created_at TIMESTAMP NOT NULL DEFAULT NOW(),
	updated_at TIMESTAMP NOT NULL DEFAULT NOW(),
	FOREIGN KEY (city_id) REFERENCES Cities(id),
	FOREIGN KEY (role_id) REFERENCES Roles(id)
);

CREATE TABLE UsersSkills (
	user_id INT NOT NULL,
	skill_id INT NOT NULL,
	PRIMARY KEY (user_id, skill_id),
	FOREIGN KEY (user_id) REFERENCES Users(id),
	FOREIGN KEY (skill_id) REFERENCES Skills(id)
);

CREATE TABLE WorkExperiences (
	id serial PRIMARY KEY,
	user_id INT NOT NULL,
	name VARCHAR(300) NOT NULL,
	start_date TIMESTAMP NOT NULL,
	end_date TIMESTAMP NULL,
	position_id INT NULL,
	FOREIGN KEY (user_id) REFERENCES Users(id),
	FOREIGN KEY (position_id) REFERENCES Positions(id)
);

ALTER TABLE Interview ADD CONSTRAINT DF_Interview_created_at DEFAULT NOW() FOR created_at;
ALTER TABLE Interview ADD CONSTRAINT DF_Interview_updated_at DEFAULT NOW() FOR updated_at;
ALTER TABLE Users ADD CONSTRAINT DF_Users_deleted DEFAULT FALSE FOR deleted;
ALTER TABLE Users ADD CONSTRAINT DF_Users_created_at DEFAULT NOW() FOR created_at;
ALTER TABLE Users ADD CONSTRAINT DF_Users_updated_at DEFAULT NOW() FOR updated_at;
```