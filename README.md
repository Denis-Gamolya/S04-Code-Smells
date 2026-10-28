# S04-Code-Smells
# Denis Gamolya, Yahir Prenger

# Long Parameter List

## What is a Long Parameter List?

A long parameter list is when a function requires so many inputs that its call becomes difficult to understand. The reader is responsible for knowing what each argument means and their order.

## How does it Arise?

As more features get added to a function, more parameters get added. Flags for selecting different behaviors also grow the parameter list.

## How to Avoid and Fix?

Keep related data together and review the function's inputs before adding parameters. 

Fowler describes these refactorings for Long Parameter Lists:

- **Introduce Parameter Object:** Create an object for values that are naturally together, such as a position.
- **Preserve Whole Object:** When several arguments come from an existing object, pass the object instead of extracting its fields at the call site.
- **Replace Parameter with Query:** If the function can get a value from another parameter, have it request that value instead of receiving both.
- **Remove Flag Argument:** Replace a parameter that selects different behaviors with clearly named functions for those behaviors.

### Before: JavaScript Example

```javascript
function createEnemy(name, x, y, health, damage, speed) {
    return { name, x, y, health, damage, speed };
}

const enemy = createEnemy("Goblin", 100, 200, 50, 10, 3);
```

### After: Introduce Parameter Object

```javascript
function createEnemy(name, position, stats) {
    return {
        name,
        x: position.x,
        y: position.y,
        health: stats.health,
        damage: stats.damage,
        speed: stats.speed
    };
}

const position = { x: 100, y: 200 };
const stats = { health: 50, damage: 10, speed: 3 };
const enemy = createEnemy("Goblin", position, stats);
```

The function now receives three parameters and named properties make the inputs easier to read.

### Before: Python Example

```python
def shipping_label(name, street, city, state, postal_code):
    return f"{name}\n{street}\n{city}, {state} {postal_code}"

address = {"street": "123 Main St", "city": "Santa Cruz", "state": "CA", "postal_code": "95060"}
label = shipping_label("Alex", address["street"], address["city"], address["state"], address["postal_code"])
```

### After: Preserve Whole Object

```python
def shipping_label(name, address):
    return f"{name}\n{address['street']}\n{address['city']}, {address['state']} {address['postal_code']}"

label = shipping_label("Alex", address)
```

Passing the existing address dictionary reduces five parameters to two.

# Long Functions

## What are Long Functions?

Lengthy methods that contain many lines and blocks of code. While they might all serve one end result, the abundance of code can confuse readers and make it harder to understand what each line achieves.

## How do they Arise?

Some programmers might find it easier to continue adding onto a function than to begin creating new functions. Especially if it's just a few lines of code, it might seem redundant or unnecessary to create a brand new functions. However, the lines begin to add up until a monstrous function exists.

## How to Avoid and Fix?

When a block of code needs to be preceded with a comment for understanding, that block can most likely be rewritten into a new function (Extract Function). 

## Javascript Example

```
update(time, delta) {
.
.
.

    //move up
    if(this.up.isDown){
        //move player sprite up
        if(my.sprite.player.y > this.upperBound){
            this.my.sprite.player.y -= this.speed * dt;
        }
    }

    //move down
    if(this.down.isDown){
        //move player sprite down
        if(my.sprite.player.y < this.lowerBound){
                this.my.sprite.player.y += this.speed * dt;
        }
    }

    //shoot a spike
    if(Phaser.Input.Keyboard.JustDown(this.spaceKey)){
        //fire spike
        // Get the first inactive spike, and make it active
        let spike = my.sprite.spikeGroup.getFirstDead();
        // spike will be null if there are no inactive (available) bullets
        if (spike != null) {
            spike.active = true;
            spike.visible = true;
            spike.x = my.sprite.player.x;
            spike.y = my.sprite.player.y;
            this.spikeThrowSound.play();
        }
    }

    //make offscreen spikes inactive
    for (let spike of my.sprite.spikeGroup.getChildren()) {
        if (spike.x > game.config.width + spike.displayWidth/2) {
            spike.active = false;
            spike.visible = false;
        }
    }

    //make offscreen bubbles inactive
    for (let bubble of my.sprite.bubbleGroup.getChildren()) {
        if (bubble.x > game.config.width + bubble.displayWidth/2) {
            bubble.active = false;
            bubble.visible = false;
        }
    }
.
.
.
}
```

## C# Example

```
public void ProcessOrder(Order order)
{
    // Validate
    if (order == null) throw new ArgumentNullException(nameof(order));
    if (!order.Items.Any()) throw new InvalidOperationException("Order has no items.");
    if (order.CustomerId <= 0) throw new InvalidOperationException("Invalid customer ID.");

    // Calculate total
    decimal subtotal = 0;
    foreach (var item in order.Items)
    {
        subtotal += item.Quantity * item.UnitPrice;
    }
    decimal tax = subtotal * 0.08m;
    decimal total = subtotal + tax;
    order.Total = total;

    // Apply discount
    if (order.CustomerIsVip)
    {
        order.Total *= 0.90m;
    }

    // Save to database
    _dbContext.Orders.Add(order);
    _dbContext.SaveChanges();

    // Send confirmation email
    var subject = $"Order #{order.Id} Confirmation";
    var body = $"Thank you for your order. Total: {order.Total:C}";
    _emailService.Send(order.CustomerEmail, subject, body);
}
```

