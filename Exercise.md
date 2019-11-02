1、

```js
function hello(){
    console.log(name);
    console.log(age);
    var name = 'd';
    let age = 12;
}
hello();
//
undefined
ReferenceError: age is not defined
```

2、

```js
const cube = {
    border : 10,
    area(){
        return this.border*this.border;
    },
    volume:()=>{
        return this.border*this.border*this.border;
    }
}
console.log(cube.area());
console.log(cube.volume());
```

3、静态方法

```js
class Chameleon {
    static colorChange(newColor) {
        this.newColor = newColor;
        return this.newColor;
    }
    constructor({newColor = 'green'} = {}){
        this.newColor = newColor;
    }
}
const freddie = new Chameleon({ newColor: 'purple'});
// console.log(freddie.colorChange('orange')); // TypeError: freddie.colorChange is not a function
console.log(Chameleon.colorChange('orange')); // orange
```

