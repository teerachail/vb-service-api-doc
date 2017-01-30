# Shopping Cart

## Get cart contents

Cloud-based cart storage so you can easily retrieve shopping carts from our API on any device.

```
curl https://api.sandbox.vb.com/v1/cart/any-unique-string \
  -H "Authorization: Bearer XXXX"
```

## Add cart items

Adding an item to a cart from your inventory or custom products is simple - just pass over the product ID, quantity, and modifiers. Our API also supports custom cart items, just provide a product name, quantity, and price.

```
curl -H "Authorization: Bearer XXXX" -X POST -d "id=6&quantity=1" https://api.sandbox.vb.com/v1/cart/1234
```

## Update a cart contents

```
curl -H "Authorization: Bearer XXXX" -X PUT -d "id=6&quantity=1" https://api.sandbox.vb.com/v1/cart/1234
```
