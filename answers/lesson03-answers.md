# Answer Key for Lesson 3

### Exercise 1 Answer:

```javascript

var billAmount = 100.58;

function gratuity(){
    return billAmount * 0.2;
  }

function totalWithGrat(amount){
  return gratuity() + amount;
}

console.log("your total, including gratuity is: $" +  totalWithGrat(billAmount).toFixed(2));
```
