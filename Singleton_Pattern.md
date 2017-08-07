```javascript
const mySingleton = (function() {
   const instance;
   
   function init() {
      // Singleton
      
      // Private members
      const privateVariable = "I'm also private";
      const privateRandomNumber = Math.random();
      
      function privateMethod() {
         console.log("I am private");
      }
      
      return {
         publicMethod: function() {
            console.log("The public can see me!");
         },
         
         publicProperty: "I'm also public",
      
         getRandomNumber: function() {
            return privateRandomNumber;
         }
      };
   };
   
   return {
      getInstance: function() {
         if (!instance) {
            instance = init();
         }
         return instance;
      }
   };
})();

// Usage:
var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
console.log( singleA.getRandomNumber() === singleB.getRandomNumber() ); // true
```
