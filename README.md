# S04-Code-Smells
# Denis Gamolya, Yahir Prenger

# Long Parameter List

# Long Functions

## What are Long Functions?

Lengthy methods that contain many lines and blocks of code. While they might all serve one end result, the abundance of code can confuse readers and make it harder to understand what each line achieves.

## How do they Arise?

Some programmers might find it easier to continue adding onto a function than to begin creating new functions. Especially if it's just a few lines of code, it might seem redundant or unnecessary to create a brand new functions. However, the lines begin to add up until a monstrous function exists.

## How to Avoid and Fix?

When a block of code needs to be preceded with a comment for understanding, that block can most likely be rewritten into a new function (Extract Function). 

## Javascript Example

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

## 

