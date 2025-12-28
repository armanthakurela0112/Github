import mysql.connector as mq

conn = mq.connect( host="localhost",user="root", passwd="12345", database="expense_tracker")
cursor = conn.cursor()
print("Welcome to Expense Tracker")
while True:
    print("\n===== MENU =====")
    print("1. Add Expense")
    print("2. View All Expenses")
    print("3. View Total Amount Spent")
    print("4. Exit")
    choice = int(input("Enter your choice: "))
    if choice == 1:
        date = input("Enter the date (DD-MM-YYYY): ")
        category = input("Enter category: ")
        description = input("Enter description: ")
        amount = float(input("Enter amount: "))

        sql = """
        INSERT INTO expenses (date, category, description, amount)
        VALUES (%s, %s, %s, %s)
        """
        values = (date, category, description, amount)
        cursor.execute(sql, values)
        conn.commit()

        print("Expense added successfully!")

   
    elif choice == 2:
        cursor.execute("SELECT * FROM expenses")
        records = cursor.fetchall()

        if len(records) == 0:
            print("No expenses added yet.")
        else:
            print("\nYour Expenses:")
            for row in records:
                print(
                    f"ID: {row[0]}, Date: {row[1]}, "
                    f"Category: {row[2]}, Description: {row[3]}, Amount: ₹{row[4]}"
                )
     
    elif choice == 3:
        cursor.execute("SELECT SUM(amount) FROM expenses")
        total = cursor.fetchone()[0]

        if total is None:
            print("Total Spending: ₹0")
        else:
            print(f"Total Spending: ₹{total}") 

    elif choice == 4:
        print("Thank you for using Expense Tracker")
        break

    else:
     print("Invalid choice, please try again")

cursor.close()
conn.close()
