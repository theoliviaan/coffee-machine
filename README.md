# coffee-machine
Coffee Machine


from menu import MENU, resources
should_continue = True

profit = 0

water = resources["water"]
milk = resources["milk"]
coffee = resources["coffee"]


def process_coins(a_quarter, a_dime, a_nickel, a_penny):
    """Returns the total calculated from coins inserted."""
    quarters = 0.25
    dimes = 0.10
    nickles = 0.05
    pennies = 0.01
    total = (quarters * a_quarter) + (dimes * a_dime) + (nickles * a_nickel) + (pennies * a_penny)
    return total


def is_resource_sufficient(order_ingredients):
    """Returns True when order can be made and false if ingredients are insufficient"""
    is_enough = True
    for item in order_ingredients:
        if order_ingredients[item] >= resources[item]:
            print(f"Sorry there is not enough {item}.")
            is_enough = False
    return is_enough


def process_change(money_received, drink_cost):
    """Makes sure the user inserted enough money"""
    change = round(money_received - drink_cost, 2)
    if money_received >= drink_cost:
        print(f"Here is your ${change} in change.")
        global profit
        profit += drink_cost
        return True

    else:
        print("Not enough money.Money refunded.")
        return False


def make_coffee(drink_name, order_ingredients):
    """Deduct the required ingredients from resources"""
    for item in order_ingredients:
        resources[item] -= order_ingredients[item]
    print(f"Here is your {drink_name} ☕. Enjoy! ")


while should_continue:
    user_choice = input("What would you like? (espresso/latte/cappuccino): ").lower()

    if user_choice == "off":
        print("Thank you for using Olive coffee machine")
        should_continue = False
    elif user_choice == "report":
        print(f"Water: {water}ml\nMilk: {milk}ml\nCoffee: {coffee}g\nMoney: ${profit}")
    else:
        drink = MENU[user_choice]
        if is_resource_sufficient(drink["ingredients"]):
            quarter = int(input("Please insert coins\nHow many quarters: "))
            dime = int(input("How many dimes: "))
            nickel = int(input("How many nickles: "))
            penny = int(input("And how many pennies: "))
            payment = process_coins(quarter, dime, nickel, penny)

            if process_change(payment, drink["cost"]):
                make_coffee(user_choice, drink["ingredients"])
