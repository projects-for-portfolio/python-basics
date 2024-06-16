# python-basics
this file contains all the python basic code 
Sure! Below are the SQL statements to create tables for a distributed database named **Programming**. We'll use SQL syntax suitable for a relational distributed database like PostgreSQL with Citus extension for sharding, which allows you to distribute tables across multiple nodes. The tables are `programming_languages`, `complexity`, `usage`, `jobs`, and `job_languages`.

### **Create Tables with SQL**

#### **1. programming_languages**
```sql
CREATE TABLE programming_languages (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    first_appeared YEAR,
    paradigm VARCHAR(255),
    designed_by VARCHAR(255)
);
```

#### **2. complexity**
```sql
CREATE TABLE complexity (
    id SERIAL PRIMARY KEY,
    language_id INTEGER REFERENCES programming_languages(id),
    complexity_level VARCHAR(50) NOT NULL,
    description TEXT
);
```

#### **3. usage**
```sql
CREATE TABLE usage (
    id SERIAL PRIMARY KEY,
    language_id INTEGER REFERENCES programming_languages(id),
    domain VARCHAR(255) NOT NULL,
    popularity_score DECIMAL(5, 2)
);
```

#### **4. jobs**
```sql
CREATE TABLE jobs (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    company VARCHAR(255) NOT NULL,
    location VARCHAR(255),
    salary_range VARCHAR(50),
    posted_date DATE
);
```

#### **5. job_languages**
```sql
CREATE TABLE job_languages (
    id SERIAL PRIMARY KEY,
    job_id INTEGER REFERENCES jobs(id),
    language_id INTEGER REFERENCES programming_languages(id)
);
```

### **Setting Up Sharding**

For a distributed database setup with PostgreSQL and Citus, you need to create the database, distribute the tables, and set the sharding key. Here’s an example:

#### **1. Initialize Citus Extension**
```sql
CREATE EXTENSION citus;
```

#### **2. Create Distributed Tables**

##### **programming_languages**
```sql
SELECT create_distributed_table('programming_languages', 'id');
```

##### **complexity**
```sql
SELECT create_distributed_table('complexity', 'id');
```

##### **usage**
```sql
SELECT create_distributed_table('usage', 'id');
```

##### **jobs**
```sql
SELECT create_distributed_table('jobs', 'id');
```

##### **job_languages**
```sql
SELECT create_distributed_table('job_languages', 'id');
```

### **Example Inserts**

#### **programming_languages**
```sql
INSERT INTO programming_languages (name, first_appeared, paradigm, designed_by)
VALUES
('Python', 1991, 'Multi-paradigm', 'Guido van Rossum'),
('Java', 1995, 'Object-oriented', 'James Gosling'),
('C++', 1985, 'Multi-paradigm', 'Bjarne Stroustrup');
```

#### **complexity**
```sql
INSERT INTO complexity (language_id, complexity_level, description)
VALUES
(1, 'Low', 'Python is known for its simple syntax and readability.'),
(2, 'Moderate', 'Java has a verbose syntax but is straightforward.'),
(3, 'High', 'C++ has complex features like pointers and manual memory management.');
```

#### **usage**
```sql
INSERT INTO usage (language_id, domain, popularity_score)
VALUES
(1, 'Web Development', 95.5),
(2, 'Enterprise Applications', 88.2),
(3, 'System Programming', 80.0);
```

#### **jobs**
```sql
INSERT INTO jobs (title, company, location, salary_range, posted_date)
VALUES
('Software Engineer', 'TechCorp', 'New York, NY', '$80,000 - $120,000', '2024-06-16'),
('Backend Developer', 'Innovate Ltd.', 'San Francisco, CA', '$100,000 - $150,000', '2024-06-15');
```

#### **job_languages**
```sql
INSERT INTO job_languages (job_id, language_id)
VALUES
(1, 1), -- Software Engineer requires Python
(2, 2); -- Backend Developer requires Java
```

### **Conclusion**

The above SQL code sets up a distributed database named **Programming** with five tables: `programming_languages`, `complexity`, `usage`, `jobs`, and `job_languages`. Each table is sharded to distribute the load across multiple nodes using the Citus extension for PostgreSQL. This setup ensures scalability and fault tolerance for managing information about programming languages, their complexity, usage, and related job listings.
