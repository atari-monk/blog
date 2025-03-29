## Write me a script in py that for a day will produce json with list of products i made that day pls. so make a obj data describing it

Sure! Here's a Python script that creates a JSON file to store the list of products you made in a day. It includes attributes like product name, quantity, production time, and any additional notes.  

Each time you run the script, it will either create a new file for the day or append new products to an existing file.

---

### Features:
- Saves data in `YYYY-MM-DD.json`
- Allows adding multiple products in a day
- Stores: `name`, `quantity`, `timestamp`, `notes`

```python
import json
import os
from datetime import datetime

# Generate filename based on today's date
today_date = datetime.now().strftime("%Y-%m-%d")
file_name = f"{today_date}.json"

# Function to load existing data or create a new structure
def load_existing_data(file_name):
    if os.path.exists(file_name):
        with open(file_name, "r") as file:
            return json.load(file)
    return {"date": today_date, "products": []}

# Function to save data to JSON
def save_data(file_name, data):
    with open(file_name, "w") as file:
        json.dump(data, file, indent=4)

# Load existing data
data = load_existing_data(file_name)

# Get product details from user
product_name = input("Enter product name: ")
quantity = int(input("Enter quantity made: "))
notes = input("Any additional notes? ")

# Create product entry
product_entry = {
    "name": product_name,
    "quantity": quantity,
    "timestamp": datetime.now().strftime("%H:%M:%S"),
    "notes": notes
}

# Append new product to data
data["products"].append(product_entry)

# Save updated data back to file
save_data(file_name, data)

print(f"Product '{product_name}' recorded successfully in {file_name}!")
```

---

### How It Works:
1. Run the script.
2. Enter the product details.
3. The script updates or creates a `YYYY-MM-DD.json` file with the product list.
4. Each entry includes time, quantity, and optional notes.

Let me know if you need any modifications! 🚀