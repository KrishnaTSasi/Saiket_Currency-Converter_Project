# Saiket_Currency-Converter_Project

# Currency Converter

A simple Python currency converter that uses the free [ExchangeRate.host API](https://exchangerate.host/) for real-time exchange rates.

## Features
- Convert between any two currencies
- Real-time exchange rates
- No API key required
- Handles invalid inputs

## Example:

```
Enter amount: 100
From currency (e.g., USD): USD
To currency (e.g., EUR): EUR
100 USD = 91.50 EUR
```

## **1. Importing the requests library**

```python
import requests
```

* `requests` is a Python package that simplifies working with APIs.

## **2. API Base URL**

```python
BASE_URL = "https://api.exchangerate.host/latest"
```

* This is the endpoint for **ExchangeRate.host** that returns the latest exchange rates.
* Example API call:

  ```
  https://api.exchangerate.host/latest?base=USD
  ```

## **3. Fetching Exchange Rates**

```python
def get_exchange_rates(base_currency):
    """Fetch exchange rates from ExchangeRate.host API."""
    try:
        response = requests.get(BASE_URL, params={"base": base_currency})
        data = response.json()
        if not data.get("success", True):
            raise Exception("API error")
        return data["rates"]
    except Exception as e:
        print(f"Error fetching exchange rates: {e}")
        return None
```

* **`params={"base": base_currency}`** tells the API which currency want as the starting point (e.g., USD).
* `.json()` converts the API’s JSON response into a Python dictionary.
* `data["rates"]` is a dictionary where the **keys are currency codes** and the **values are exchange rates**.
* Error handling:

  * The API fails, we catch the exception and return `None`.

## **4. Converting Currency**

```python
def currency_converter(amount, from_currency, to_currency):
    """Convert currency using real-time rates."""
    rates = get_exchange_rates(from_currency)
    if rates is None:
        return None
    
    if to_currency not in rates:
        print("Invalid target currency code.")
        return None
    
    converted_amount = amount * rates[to_currency]
    return converted_amount
```

* First, get rates for the **source currency** (`from_currency`).
* If the **target currency** (`to_currency`) isn’t in the rates, show an error.
* Conversion formula:

  ```
  converted_amount = amount × rate_for_target_currency
  ```

## **5. User Interaction (Main Program)**

```python
if __name__ == "__main__":
    print("=== Currency Converter ===")
    try:
        amount = float(input("Enter amount: "))
        from_currency = input("From currency (e.g., USD): ").upper()
        to_currency = input("To currency (e.g., EUR): ").upper()
        
        result = currency_converter(amount, from_currency, to_currency)
        if result is not None:
            print(f"{amount} {from_currency} = {result:.2f} {to_currency}")
    except ValueError:
        print("Invalid amount entered.")
```

* `input()` asks the user for:

  * Amount to convert
  * From currency code
  * To currency code
* `.upper()` ensured currency codes are uppercase (`usd` → `USD`).
* The conversion works, print the result with **two decimal places**.
* The amount isn’t a valid number, a `ValueError` is caught.







