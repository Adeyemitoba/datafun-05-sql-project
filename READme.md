datafun-05-sql-project

PROJECT 5 REPOSITORY

AIM: Develop a Python showcasing the capability to interact with a SQL database, interacting with tasks such as database creation, schema definition, and execution of diverse SQL commands

STEPS TAKEN
1.	Start a new project repository in GitHub with default ReadME
2.	Clone down to local machine
3.	Open project in VS Code
4.	Create and activate Virtual Environment
py -m venv .venv
.venv\Scripts\Activat
5.	Add requirements 
requirements.txt
or py -m pip install -r requirements.txt.
6.	Add gitignore and with .vscode/ and .venv/.
Gitignore
7.	Add dependencies
py -m pip install pandas
py -m pip install pyarrow
python.exe -m pip install --upgrade pip
8.	Freeze dependencies
py -m pip freeze > requirements.txt
9.	Git add and commit
git add .
git commit -m "add .gitignore, cmds to readme"
git push origin main
The script performs the following SQL operations:
•	Create Tables: Defines the structure of tables in the database.
•	Insert Records: Inserts data into tables from CSV files.
•	Update Records: Modifies existing records in the tables.
•	Delete Records: Removes records from the tables.
•	Query Aggregation: Performs aggregate functions (e.g., SUM, AVG) on the data.
•	Query Filtering: Retrieves specific records based on specified conditions.
•	Query Sorting: Orders the retrieved records based on specified criteria.
•	Query Grouping: Groups records based on specified attributes.
•	Query Joining: Combines data from multiple tables based on specified relationships.


CREATE DATABASE USING THE PYTHON FILE

db_file = pathlib.Path("project.db")

def create_database():
    """Function to create a database. Connecting for the first time
    will create a new database file if it doesn't exist yet.
    Close the connection after creating the database
    to avoid locking the file."""
    try:
        conn = sqlite3.connect(db_file)
        conn.close()
        print("Database created successfully.")
    except sqlite3.Error as e:
        print("Error creating the database:", e)


CREATE SQL SCRIPTS IN SQL_FILE FOLDER


def create_tables():
    """Function to read and execute SQL statements to create tables"""
    try:
        with sqlite3.connect(db_file) as conn:
            sql_file = pathlib.Path("sql", "create_tables.sql")
            with open(sql_file, "r") as file:
                sql_script = file.read()
            conn.executescript(sql_script)
            print("Tables created successfully.")
    except sqlite3.Error as e:
        print("Error creating tables:", e)

def insert_data_from_csv():
    """Function to use pandas to read data from CSV files (in 'data' folder)
    and insert the records into their respective tables."""
    try:
        author_data_path = pathlib.Path("data", "authors.csv")
        book_data_path = pathlib.Path("data", "books.csv")
        authors_df = pd.read_csv(author_data_path)
        books_df = pd.read_csv(book_data_path)
        with sqlite3.connect(db_file) as conn:
            # use the pandas DataFrame to_sql() method to insert data
            # pass in the table name and the connection
            authors_df.to_sql("authors", conn, if_exists="replace", index=False)
            books_df.to_sql("books", conn, if_exists="replace", index=False)
            print("Data inserted successfully.")
    except (sqlite3.Error, pd.errors.EmptyDataError, FileNotFoundError) as e:
        print("Error inserting data:", e)



create_tables()
    insert_data_from_csv()

if __name__ == "__main__":
    main()


