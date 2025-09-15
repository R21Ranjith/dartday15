import 'dart:io';

// ------------------ MODEL CLASS ------------------
class Item {
  int id;
  String name;
  int quantity;
  double price;

  Item(this.id, this.name, this.quantity, this.price);

  void display() {
    print("ID: $id | Name: $name | Qty: $quantity | Price: ₹$price");
  }
}

// ------------------ INVENTORY MANAGER ------------------
class Inventory {
  final Map<int, Item> _items = {};

  void addItem(Item item) {
    if (_items.containsKey(item.id)) {
      print("⚠️ Item with ID ${item.id} already exists!");
    } else {
      _items[item.id] = item;
      print("✅ Item added successfully!");
    }
  }

  void updateItem(int id, {String? name, int? qty, double? price}) {
    if (_items.containsKey(id)) {
      var item = _items[id]!;
      if (name != null) item.name = name;
      if (qty != null) item.quantity = qty;
      if (price != null) item.price = price;
      print("✅ Item updated successfully!");
    } else {
      print("❌ Item not found!");
    }
  }

  void deleteItem(int id) {
    if (_items.remove(id) != null) {
      print("🗑️ Item deleted successfully!");
    } else {
      print("❌ Item not found!");
    }
  }

  void searchItem(int id) {
    if (_items.containsKey(id)) {
      _items[id]!.display();
    } else {
      print("❌ Item not found!");
    }
  }

  void viewAll() {
    if (_items.isEmpty) {
      print("📦 Inventory is empty!");
    } else {
      print("\n--- Inventory List ---");
      _items.values.forEach((item) => item.display());
    }
  }
}

// ------------------ MAIN PROGRAM ------------------
void main() {
  Inventory inventory = Inventory();

  while (true) {
    print("\n=== Inventory Manager ===");
    print("1. Add Item");
    print("2. Update Item");
    print("3. Delete Item");
    print("4. Search Item");
    print("5. View All Items");
    print("6. Exit");
    stdout.write("Enter choice: ");

    String? choice = stdin.readLineSync();
    switch (choice) {
      case '1':
        try {
          stdout.write("Enter ID: ");
          int id = int.parse(stdin.readLineSync()!);
          stdout.write("Enter Name: ");
          String name = stdin.readLineSync()!;
          stdout.write("Enter Quantity: ");
          int qty = int.parse(stdin.readLineSync()!);
          stdout.write("Enter Price: ");
          double price = double.parse(stdin.readLineSync()!);

          inventory.addItem(Item(id, name, qty, price));
        } catch (e) {
          print("⚠️ Invalid input! Try again.");
        }
        break;

      case '2':
        try {
          stdout.write("Enter ID to update: ");
          int id = int.parse(stdin.readLineSync()!);
          stdout.write("Enter new Name (or press Enter to skip): ");
          String? name = stdin.readLineSync();
          stdout.write("Enter new Quantity (or press Enter to skip): ");
          String? qtyInput = stdin.readLineSync();
          stdout.write("Enter new Price (or press Enter to skip): ");
          String? priceInput = stdin.readLineSync();

          inventory.updateItem(
            id,
            name: name!.isEmpty ? null : name,
            qty: qtyInput!.isEmpty ? null : int.tryParse(qtyInput),
            price: priceInput!.isEmpty ? null : double.tryParse(priceInput),
          );
        } catch (e) {
          print("⚠️ Invalid input!");
        }
        break;

      case '3':
        stdout.write("Enter ID to delete: ");
        int id = int.parse(stdin.readLineSync()!);
        inventory.deleteItem(id);
        break;

      case '4':
        stdout.write("Enter ID to search: ");
        int id = int.parse(stdin.readLineSync()!);
        inventory.searchItem(id);
        break;

      case '5':
        inventory.viewAll();
        break;

      case '6':
        print("👋 Exiting Inventory Manager. Goodbye!");
        return;

      default:
        print("⚠️ Invalid choice! Please try again.");
    }
  }
}
