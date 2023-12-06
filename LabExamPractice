from abc import ABC, abstractmethod
from enum import Enum
from collections import deque

MENU = {
    'Burger': {'Price': 13.25, 'Type': 'Entree'},
    'Pizza': {'Price': 21.50, 'Type': 'Entree'},
    'Mozzarella Sticks': {'Price': 5.75, 'Type': 'Appetizer'},
    'Jalapeno Poppers': {'Price': 4.50, 'Type': 'Appetizer'},
    'Icecream Sundae': {'Price': 7.00, 'Type': 'Dessert'},
    'Fudge Brownie': {'Price': 3.50, 'Type': 'Dessert'},
    'Limeade': {'Price': 3.00, 'Type': 'Drink'},
    'Water': {'Price': 0.00, 'Type': 'Drink'},
    'Soda': {'Price': 1.50, 'Type': 'Drink'}
}

class Priority(Enum):
    URGENT = 0
    IMMEDIATE = 1
    NORMAL = 2
    DELAYED = 3

class Food(ABC):
    def __init__(self, name: str, base_price: float, table: int):
        self.name = name
        self.base_price = base_price
        self.current_price = base_price
        self.table_number = table

    @abstractmethod
    def get_name(self):
        pass

    @abstractmethod
    def get_price(self):
        pass

    def __str__(self):
        return f'Item: {self.name}\n\tPrice: ${self.current_price:.2f}'

    def increase_priority(self):
        pass

    def decrease_priority(self):
        pass

class Appetizer(Food):
    def __init__(self, name: str, base_price: float, table: int):
        super().__init__(name, base_price, table)
        self.priority = Priority.IMMEDIATE

    def get_name(self):
        return self.name

    def get_price(self):
        return self.current_price

    def increase_priority(self):
        if self.priority == Priority.URGENT:
            print("Priority is already at the maximum (URGENT)")
        else:
            current_priority = self.priority.value
            self.priority = Priority(current_priority - 1)

    def decrease_priority(self):
        if self.priority == Priority.DELAYED:
            print("Priority is already at the minimum (DELAYED)")
        else:
            current_priority = self.priority.value
            self.priority = Priority(current_priority + 1)

class Entree(Food):
    def __init__(self, name: str, base_price: float, table: int):
        super().__init__(name, base_price, table)
        self.priority = Priority.NORMAL

    def get_name(self):
        return self.name

    def get_price(self):
        return self.current_price

    def increase_priority(self):
        if self.priority == Priority.URGENT:
            print("Priority is already at the maximum (URGENT)")
        else:
            current_priority = self.priority.value
            self.priority = Priority(current_priority - 1)

    def decrease_priority(self):
        if self.priority == Priority.DELAYED:
            print("Priority is already at the minimum (DELAYED)")
        else:
            current_priority = self.priority.value
            self.priority = Priority(current_priority + 1)

class Dessert(Food):
    def __init__(self, name: str, base_price: float, table: int):
        super().__init__(name, base_price, table)
        self.priority = Priority.DELAYED

    def get_name(self):
        return self.name

    def get_price(self):
        return self.current_price

    def increase_priority(self):
        if self.priority == Priority.URGENT:
            print("Priority is already at the maximum (URGENT)")
        else:
            current_priority = self.priority.value
            self.priority = Priority(current_priority - 1)

    def decrease_priority(self):
        if self.priority == Priority.DELAYED:
            print("Priority is already at the minimum (DELAYED)")
        else:
            current_priority = self.priority.value
            self.priority = Priority(current_priority + 1)

class Drink(Food):
    def __init__(self, name: str, base_price: float, table: int, is_happy_hour: bool = False):
        super().__init__(name, base_price, table)
        self.priority = Priority.IMMEDIATE
        self.is_happy_hour = is_happy_hour

    def get_name(self):
        return self.name

    def get_price(self):
        if self.is_happy_hour:
            return self.current_price * 0.5  # Apply 50% discount during happy hour
        else:
            return self.current_price

class Table:
    def __init__(self, table_number: int):
        self.table_number = table_number
        self.food_types = {'Appetizer': [], 'Entree': [], 'Dessert': [], 'Drink': []}
        self.food_items = []
        self.raw_order = []  # Keeps track of raw orders

    def add_to_table(self, food_item: Food):
        self.food_items.append(food_item)

        # Categorize the food item based on its type
        if isinstance(food_item, Appetizer):
            self.food_types['Appetizer'].append(food_item)
        elif isinstance(food_item, Entree):
            self.food_types['Entree'].append(food_item)
        elif isinstance(food_item, Dessert):
            self.food_types['Dessert'].append(food_item)
        elif isinstance(food_item, Drink):
            self.food_types['Drink'].append(food_item)

        # Add to raw_order list
        self.raw_order.append(food_item)

    def take_order(self, item_name: str):
        # Taking order by referencing the menu dictionary
        if item_name in MENU:
            item_info = MENU[item_name]
            food_type = item_info['Type']
            price = item_info['Price']

            if food_type == 'Appetizer':
                new_order = Appetizer(item_name, price, self.table_number)
            elif food_type == 'Entree':
                new_order = Entree(item_name, price, self.table_number)
            elif food_type == 'Dessert':
                new_order = Dessert(item_name, price, self.table_number)
            elif food_type == 'Drink':
                new_order = Drink(item_name, price, self.table_number)

            self.add_to_table(new_order)
            print(f"{item_name} added to Table {self.table_number}'s order.")
        else:
            print(f"{item_name} is not available in the menu.")

    def subtotal(self):
        return sum(item.get_price() for item in self.food_items)

    def print_check(self):
        print(f"Check for Table {self.table_number}:")
        print("----------------------------")
        for item in self.food_items:
            print(f"{item.get_name()} - ${item.get_price():.2f}")
        subtotal = self.subtotal()
        tax = subtotal * 0.1  # Assuming a tax rate of 10%
        total = subtotal + tax
        print(f"Subtotal: ${subtotal:.2f}")
        print(f"Tax: ${tax:.2f}")
        print(f"Total: ${total:.2f}")
        print("----------------------------")

class Kitchen:
    def __init__(self):
        self.orders_queue = deque()

    def add_to_line(self, table: Table):
        self.orders_queue.append(table)

    def remove_order(self, item_name: str, table_number: int):
        for table in self.orders_queue:
            if table.table_number == table_number:
                for item in table.food_items:
                    if item.get_name() == item_name:
                        table.food_items.remove(item)
                        table.raw_order.remove(item)
                        print(f"Removed {item_name} from Table {table_number}'s order.")
                        return
                print(f"{item_name} not found in Table {table_number}'s order.")
                return
        print(f"Table {table_number} not found in the kitchen.")

    def cook(self):
        if self.orders_queue:
            table = self.orders_queue[0]
            next_item = table.food_items[0]
            table.food_items.remove(next_item)
            print(f"Preparing {next_item.get_name()} for Table {table.table_number}.")

    def increment_priority(self):
        for table in self.orders_queue:
            for item in table.food_items:
                item.increase_priority()

    def sort_queue_by_priority(self):
        def get_min_priority(table):
            if table.food_items:
                return min(item.priority.value for item in table.food_items)
            return float('inf')  # Return a high value if no items in table

        self.orders_queue = deque(sorted(self.orders_queue, key=get_min_priority))

# Example usage for Table and Kitchen classes
table1 = Table(1)
table1.take_order('Mozzarella Sticks')
table1.take_order('Pizza')
table1.take_order('Fudge Brownie')
table1.take_order('Soda')
table1.print_check()

kitchen = Kitchen()
kitchen.add_to_line(table1)
kitchen.cook()
kitchen.increment_priority()
kitchen.sort_queue_by_priority()
