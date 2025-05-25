# lucky-modifications
All the madness happens here...
```python name=app.py
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

# In-memory store, for demonstration purposes only
products = [
    {
        'id': 1,
        'name': 'Custom Alloy Wheels',
        'price': 1200,
        'description': 'High performance alloy wheels for better grip and style.',
        'image': 'wheels.jpg'
    },
    {
        'id': 2,
        'name': 'Performance Exhaust',
        'price': 800,
        'description': 'Upgrade your car sound and performance.',
        'image': 'exhaust.jpg'
    },
    {
        'id': 3,
        'name': 'Sport Suspension',
        'price': 1500,
        'description': 'Lower your car and improve handling.',
        'image': 'suspension.jpg'
    },
]

cart = []

@app.route('/')
def index():
    return render_template('index.html', products=products)

@app.route('/product/<int:product_id>')
def product_detail(product_id):
    product = next((p for p in products if p['id'] == product_id), None)
    return render_template('product.html', product=product)

@app.route('/add_to_cart/<int:product_id>')
def add_to_cart(product_id):
    product = next((p for p in products if p['id'] == product_id), None)
    if product:
        cart.append(product)
    return redirect(url_for('cart_view'))

@app.route('/cart')
def cart_view():
    total = sum(item['price'] for item in cart)
    return render_template('cart.html', cart=cart, total=total)

@app.route('/checkout')
def checkout():
    cart.clear()
    return render_template('checkout.html')

if __name__ == '__main__':
    app.run(debug=True)
```

```html name=templates/index.html
<!DOCTYPE html>
<html>
<head>
    <title>Lucky Modifications - Car Mod Store</title>
</head>
<body>
    <h1>Welcome to Lucky Modifications</h1>
    <h2>Available Products</h2>
    <ul>
        {% for product in products %}
        <li>
            <img src="{{ product.image }}" width="150">
            <h3><a href="{{ url_for('product_detail', product_id=product.id) }}">{{ product.name }}</a></h3>
            <p>{{ product.description }}</p>
            <strong>${{ product.price }}</strong>
            <br>
            <a href="{{ url_for('add_to_cart', product_id=product.id) }}">Add to Cart</a>
        </li>
        {% endfor %}
    </ul>
    <p><a href="{{ url_for('cart_view') }}">View Cart</a></p>
</body>
</html>
```

```html name=templates/product.html
<!DOCTYPE html>
<html>
<head>
    <title>{{ product.name }} - Lucky Modifications</title>
</head>
<body>
    <h1>{{ product.name }}</h1>
    <img src="{{ product.image }}" width="300">
    <p>{{ product.description }}</p>
    <strong>Price: ${{ product.price }}</strong>
    <br>
    <a href="{{ url_for('add_to_cart', product_id=product.id) }}">Add to Cart</a>
    <br>
    <a href="{{ url_for('index') }}">Back to Products</a>
</body>
</html>
```

```html name=templates/cart.html
<!DOCTYPE html>
<html>
<head>
    <title>Shopping Cart - Lucky Modifications</title>
</head>
<body>
    <h1>Your Cart</h1>
    {% if cart %}
        <ul>
        {% for item in cart %}
            <li>
                <img src="{{ item.image }}" width="100">
                {{ item.name }} - ${{ item.price }}
            </li>
        {% endfor %}
        </ul>
        <strong>Total: ${{ total }}</strong>
        <br>
        <a href="{{ url_for('checkout') }}">Checkout</a>
    {% else %}
        <p>Your cart is empty.</p>
    {% endif %}
    <br>
    <a href="{{ url_for('index') }}">Continue Shopping</a>
</body>
</html>
```

```html name=templates/checkout.html
<!DOCTYPE html>
<html>
<head>
    <title>Checkout - Lucky Modifications</title>
</head>
<body>
    <h1>Thank you for your purchase!</h1>
    <p>Your order has been placed successfully.</p>
    <a href="{{ url_for('index') }}">Back to Home</a>
</body>
</html>
```

````markdown name=README.md
# Lucky Modifications Store

A simple car modifications store website built with Flask.

## Features

- Browse car modification products
- View product details
- Add products to cart
- Checkout

## Running the App

1. Install dependencies:
    ```
    pip install Flask
    ```
2. Place the images (`wheels.jpg`, `exhaust.jpg`, `suspension.jpg`) in a `static/` folder.
3. Run the app:
    ```
    python app.py
    ```
4. Open [http://localhost:5000](http://localhost:5000) in your browser.

````
Let me know if you need further customization or a more advanced setup!
