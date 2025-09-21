import csv

def calculate_daily_earnings():
    """
    Calculate the total daily earnings from the CSV file by multiplying
    cost by quantity for each item and summing the results.
    Uses dictionary approach with two different casting methods.
    """
    total_earnings = 0
    
    try:        
        # Read the CSV file using dictionary approach
        with open('Day54Totals.csv', 'r') as file:
            # Use DictReader to read CSV as dictionaries
            reader = csv.DictReader(file)
            
            # Check what columns are available
            columns = reader.fieldnames
            print(f"Found columns: {columns}")
            
            # Try to find the cost and quantity columns with different possible names
            cost_column = None
            quantity_column = None
            
            # Check for possible cost column names
            for col in columns:
                col_lower = col.lower().strip()
                if col_lower in ['cost', 'price', 'unit_price', 'unitprice']:
                    cost_column = col
                    break
            
            # Check for possible quantity column names
            for col in columns:
                col_lower = col.lower().strip()
                if col_lower in ['quantity', 'qty', 'amount', 'count', 'number']:
                    quantity_column = col
                    break
            
            if not cost_column or not quantity_column:
                print(f"Could not find required columns!")
                print(f"Looking for cost column (found: {cost_column})")
                print(f"Looking for quantity column (found: {quantity_column})")
                return None
            
            print(f"Using '{cost_column}' for cost and '{quantity_column}' for quantity")
            print()
            
            # Convert the reader to a list to read all data while file is open
            rows = list(reader)
        
        # Now process each row (file is closed but data is in memory)
        for row in rows:
            # Take in 2 different values from the dictionary
            cost_value = row[cost_column]
            quantity_value = row[quantity_column]
            
            # Strip any whitespace that might cause issues
            cost_value = cost_value.strip()
            quantity_value = quantity_value.strip()
            
            # Cast them in 2 different ways:
            # 1. Cost as float (for decimal currency values)
            cost = float(cost_value)
            # 2. Quantity as integer (for whole numbers of items)
            quantity = int(quantity_value)
            
            # Calculate earnings for this item (cost × quantity)
            item_earnings = cost * quantity
            total_earnings += item_earnings
            
            print(f"Item: £{cost:.2f} × {quantity} = £{item_earnings:.2f}")
    
    except FileNotFoundError:
        print("Error: Day54Totals.csv file not found!")
        print("Make sure the file is in the same directory as this script.")
        return None
    except ValueError as e:
        print(f"Error: Invalid data in CSV file - {e}")
        print("Check that cost and quantity values are valid numbers.")
        return None
    except Exception as e:
        print(f"Error reading CSV file: {e}")
        return None
    
    return total_earnings

def inspect_csv_file():
    """Inspect the CSV file to see what columns it actually has"""
    try:
        with open('Day54Totals.csv', 'r') as file:
            reader = csv.DictReader(file)
            print("Available columns in CSV file:")
            print(list(reader.fieldnames))
            print("\nFirst few rows of data:")
            for i, row in enumerate(reader):
                if i < 3:  # Show first 3 rows
                    print(f"Row {i+1}: {dict(row)}")
                else:
                    break
    except FileNotFoundError:
        print("CSV file not found!")

def main():
    """Main function to run the earnings calculator"""
    print("🌟Shop $ Tracker🌟")
    print("\nInspecting CSV file structure...")
    inspect_csv_file()
    print("\n" + "="*50 + "\n")
    
    earnings = calculate_daily_earnings()
    
    if earnings is not None:
        # Display result in the requested format
        print(f"Your shop took £{earnings:.0f} pounds today.")

# Alternative version that works with sample data for demonstration
def demo_with_sample_data():
    """Demo function with sample data in case CSV file isn't available"""
    print("🌟Shop $$ Tracker🌟")
    print()
    print("Demo with sample data:")
    
    # Sample data structure similar to what might be in the CSV
    sample_data = [
        {'cost': '12.50', 'quantity': '5'},
        {'cost': '8.99', 'quantity': '12'},
        {'cost': '15.00', 'quantity': '8'},
        {'cost': '3.25', 'quantity': '20'},
        {'cost': '25.99', 'quantity': '3'}
    ]
    
    total_earnings = 0
    
    for row in sample_data:
        # Take in 2 different values
        cost_value = row['cost']
        quantity_value = row['quantity']
        
        # Cast them in 2 different ways
        cost = float(cost_value)  # Cast as float
        quantity = int(quantity_value)  # Cast as integer
        
        # Calculate earnings
        item_earnings = cost * quantity
        total_earnings += item_earnings
        
        print(f"Item: £{cost:.2f} × {quantity} = £{item_earnings:.2f}")
    
    print(f"\nYour shop took £{total_earnings:.0f} pounds today.")

if __name__ == "__main__":
    # Try to run with actual CSV file first
    main()
    
    # If you want to see the demo with sample data, uncomment the line below:
    # demo_with_sample_data()
