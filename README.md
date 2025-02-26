# basic-client-database
Step-by-Step Guide
Install SQLite3: SQLite3 is included with Python, so you don't need to install it separately.
Create a Python Script: Below is a Python script that will create a SQLite database and allow for basic CRUD (Create, Read, Update, Delete) operations.
Python Script
import sqlite3

# Connect to SQLite database (it will create a new database if it doesn't exist)
conn = sqlite3.connect('clients.db')

# Create a cursor object to interact with the database
cur = conn.cursor()

# Create a table for storing client data
cur.execute('''
CREATE TABLE IF NOT EXISTS clients (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE,
    phone TEXT,
    address TEXT
)
''')

# Function to add a new client
def add_client(name, email, phone, address):
    try:
        cur.execute('''
        INSERT INTO clients (name, email, phone, address)
        VALUES (?, ?, ?, ?)
        ''', (name, email, phone, address))
        conn.commit()
        print("Client added successfully!")
    except sqlite3.IntegrityError as e:
        print(f"Error: {e}")

# Function to update client information
def update_client(client_id, name=None, email=None, phone=None, address=None):
    try:
        if name:
            cur.execute('UPDATE clients SET name = ? WHERE id = ?', (name, client_id))
        if email:
            cur.execute('UPDATE clients SET email = ? WHERE id = ?', (email, client_id))
        if phone:
            cur.execute('UPDATE clients SET phone = ? WHERE id = ?', (phone, client_id))
        if address:
            cur.execute('UPDATE clients SET address = ? WHERE id = ?', (address, client_id))
        conn.commit()
        print("Client updated successfully!")
    except sqlite3.IntegrityError as e:
        print(f"Error: {e}")

# Function to retrieve client information
def get_client(client_id):
    cur.execute('SELECT * FROM clients WHERE id = ?', (client_id,))
    client = cur.fetchone()
    if client:
        print(client)
    else:
        print("Client not found.")

# Function to delete a client
def delete_client(client_id):
    cur.execute('DELETE FROM clients WHERE id = ?', (client_id,))
    conn.commit()
    print("Client deleted successfully!")

# Example usage
if __name__ == "__main__":
    # Add a new client
    add_client("John Doe", "john.doe@example.com", "555-1234", "123 Elm Street")

    # Update an existing client
    update_client(1, phone="555-5678", address="456 Oak Street")

    # Retrieve client information
    get_client(1)

    # Delete a client
    delete_client(1)

# Close the connection
conn.close()
Explanation
Create Database and Table:

The script connects to a SQLite database named clients.db.
It creates a table clients if it doesn't already exist, with columns for id, name, email, phone, and address.
Add Client:

The add_client function inserts a new client into the database.
The email field is unique to avoid duplicates.
Update Client:

The update_client function updates existing client information based on the provided client_id.
Retrieve Client:

The get_client function retrieves and prints the details of a client based on the client_id.
Delete Client:

The delete_client function deletes a client from the database based on the client_id.
Running the Script
Save the script to a file named client_database.py.
Run the script using the command: python client_database.py.
gooogle
