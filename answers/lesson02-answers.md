# Answer Key for Lesson 2

### Exercise 1 answer

```javascript
var list = ['pop tarts', 'ramen noodles', 'chips', 'salsa', 'coffee'];
list[4] = 'fair trade coffee';
list.push('fruit loops');
list.splice(2,2,'rice', 'beans');

var cart = [];
cart.push(list.pop());
cart.push(list.shift());

while(list.length > 0)
    cart.push(list.pop());

cart.sort().reverse();
console.log("My cart contains: ", cart.join(','));
```

### Exercise 2: Addressing Objects Answer

1. course.name;
2. course.teachers[1];
3. course.students[0].name;
4. course.students[1].computer.type;
5. course.preReqs.equipment
6. course.preReqs.equipment.OSOptions[1];
7. course.preReqs.equipment.OSOptions.join(' or ');
8.
```javascript
var OSXStudents = [];
for(var i = 0; i < course.students.length; i++) {
 if(course.students[i].computer.OS === 'OSX')
 	OSXStudents.push(course.students[i]);
}
console.log(OSXStudents);
```
