

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
  body{
      background:silver;
      overflow:hidden;
  }
    .bullet {
      position: absolute;
      width: 25px;
      height: 15px;
      top: 50%;
      border:none;
      text-decoration:none; background:url('https://nagasasank.files.wordpress.com/2024/01/bullet-gun-bullet-pointed-images-3.png?w=768');
      background-size:contain;
      transform: translate(-50%, -50%);
      transform:rotate(90deg);
    }
    button {
      user-select: none;
      border:5px ridge black;
      border-radius:20px;
    }
    #car-health {
      --meter-accent-color: green; /* Set your desired accent color */
    }
    #tank-health {
      --meter-accent-color: red; /* Set your desired accent color */
    }
    meter::-webkit-meter-optimum-value {
      background: var(--meter-accent-color);
    }
    #tank {
      position: relative;
      left: 0;
      top: 0;
      width: 100px;
      height: 90px;
      transform: rotate(45deg);
      background: transparent;
      transition: left 0.5s, top 0.5s;
    }
    #car {
      position: relative;
      left: 0;
      top: 0;
      width: 60px;
      height: 50px;
      background: transparent;
      transition: left 0.5s, top 0.5s;
    }
    #game-container {
      position: relative;
      width: 340px;
      height: 450px;
      border: 1px solid black;
      overflow: hidden;
      background:url('https://nagasasank.files.wordpress.com/2024/01/download282129.jpeg');
      background-size:contain;
    }
  </style>
  <script>
    document.addEventListener('DOMContentLoaded', function () {
      var gameContainer;
      var bullet;
      var xSpeed, ySpeed;

      var tank = document.getElementById('tank');
      var car = document.getElementById('car');
      gameContainer = document.getElementById('game-container');
      var carHealthMeter = document.getElementById('car-health');

      function moveCar(direction) {
        var currentLeft = parseInt(window.getComputedStyle(car).left);
        var currentTop = parseInt(window.getComputedStyle(car).top);
        var step = 100;

        switch (direction) {
          case 'up':
            car.style.top = Math.max(currentTop - step, 0) + 'px';
            break;
          case 'down':
            car.style.top = Math.min(currentTop + step, gameContainer.clientHeight - car.clientHeight) + 'px';
            break;
          case 'left':
            car.style.left = Math.max(currentLeft - step, 0) + 'px';
            break;
          case 'right':
            car.style.left = Math.min(currentLeft + step, gameContainer.clientWidth - car.clientWidth) + 'px';
            break;
        }
      }

      function shootBulletFromCar() {
        bullet = document.createElement('img');
        bullet.className = 'bullet';
        gameContainer.appendChild(bullet);

        var bulletSpeed = 50;
        var carRect = car.getBoundingClientRect();
        var carRotation = parseInt(car.style.transform.replace('rotate(', '').replace('deg)', ''));
        var angle = (carRotation * Math.PI) / 180;

        xSpeed = bulletSpeed * Math.cos(angle);
        ySpeed = bulletSpeed * Math.sin(angle);

        bullet.style.left = carRect.left + car.clientWidth / 2 + 'px';
        bullet.style.top = carRect.top + car.clientHeight / 2 + 'px';

        function moveBullet() {
          var bulletRect = bullet.getBoundingClientRect();
          bullet.style.left = bulletRect.left + xSpeed + 'px';
          bullet.style.top = bulletRect.top + ySpeed + 'px';

          // Check collision with tank
          var tankRect = tank.getBoundingClientRect();
          if (isColliding(bulletRect, tankRect)) {
            decreaseTankHealth();
            gameContainer.removeChild(bullet);
            return;
          }

          // Check if bullet is out of bounds
          if (
            bulletRect.left < 0 ||
            bulletRect.top < 0 ||
            bulletRect.right > gameContainer.clientWidth ||
            bulletRect.bottom > gameContainer.clientHeight
          ) {
            gameContainer.removeChild(bullet);
          } else {
            requestAnimationFrame(moveBullet);
          }
        }

        requestAnimationFrame(moveBullet);
      }

      function shootBulletFromTank() {
        // Code for shooting bullets from the tank
        bullet = document.createElement('img');
        bullet.className = 'bullet';
        gameContainer.appendChild(bullet);

        var bulletSpeed = 50;
        var tankRect = tank.getBoundingClientRect();
        var carRect = car.getBoundingClientRect();

        var angle = Math.atan2(carRect.top - tankRect.top, carRect.left - tankRect.left);
        xSpeed = bulletSpeed * Math.cos(angle);
        ySpeed = bulletSpeed * Math.sin(angle);

        bullet.style.left = tankRect.left + tank.clientWidth / 2 + 'px';
        bullet.style.top = tankRect.top + tank.clientHeight / 2 + 'px';

        function moveBullet() {
          var bulletRect = bullet.getBoundingClientRect();
          bullet.style.left = bulletRect.left + xSpeed + 'px';
          bullet.style.top = bulletRect.top + ySpeed + 'px';

          // Check collision with car
          var carRect = car.getBoundingClientRect();
          if (isColliding(bulletRect, carRect)) {
            decreaseCarHealth();
            gameContainer.removeChild(bullet);
            return;
          }

          // Check if bullet is out of bounds
          if (
            bulletRect.left < 0 ||
            bulletRect.top < 0 ||
            bulletRect.right > gameContainer.clientWidth ||
            bulletRect.bottom > gameContainer.clientHeight
          ) {
            gameContainer.removeChild(bullet);
          } else {
            requestAnimationFrame(moveBullet);
          }
        }

        requestAnimationFrame(moveBullet);
      setTimeout(moveTank, ab); // Adjust the delay as needed
}


moveTank();

      function moveTank() {
        // Code for moving the tank automatically and shooting
        var directions = ['up', 'down', 'left', 'right'];
        var randomDirection = directions[Math.floor(Math.random() * directions.length)];

        var currentLeft = parseInt(window.getComputedStyle(tank).left);
        var currentTop = parseInt(window.getComputedStyle(tank).top);
        var step = 80;

        switch (randomDirection) {
          case 'up':
            tank.style.transform = 'rotate(225deg)';
            tank.style.top = Math.max(currentTop - step, 0) + 'px';
            break;
          case 'down':
            tank.style.transform = 'rotate(45deg)';
            tank.style.top = Math.min(currentTop + step, gameContainer.clientHeight - tank.clientHeight) + 'px';
            break;
          case 'left':
            tank.style.transform = 'rotate(135deg)';
            tank.style.left = Math.max(currentLeft - step, 0) + 'px';
            break;
          case 'right':
            tank.style.transform = 'rotate(315deg)';
            tank.style.left =
              Math.min(currentLeft + step, gameContainer.clientWidth - tank.clientWidth) + 'px';
            break;
        }

        // Automatically shoot at the car
        shootBulletFromTank();
      }

      setInterval(moveTank, 3000);

      // Event listeners for moving the car
      document.getElementById('btn-up').addEventListener('click', function () {
        
        car.style.transform = 'rotate(0deg)';
        moveCar('up');
      });

      document.getElementById('btn-down').addEventListener('click', function () {
        car.style.transform = 'rotate(180deg)';
        moveCar('down');
      });

      document.getElementById('btn-left').addEventListener('click', function () {
        car.style.transform = 'rotate(-90deg)';
        moveCar('left');
      });

      document.getElementById('btn-right').addEventListener('click', function () {
        car.style.transform=
        'rotate(90deg)';
        moveCar('right');
        });
      document.getElementById('btn-shoot-car').addEventListener('click', function () {
        car.style.transform = 'rotate(180deg)';
        shootBulletFromCar();
      });

      function isColliding(rect1, rect2) {
        // Code for checking collision between two rectangles
        // Return true if colliding, false otherwise
        return !(
          rect1.right < rect2.left ||
          rect1.left > rect2.right ||
          rect1.bottom < rect2.top ||
          rect1.top > rect2.bottom
        );
      }
    
    function checkGameStatus() {
  if (carHealthMeter.value <= 0) {
    alert('Game Over! Your car has been destroyed.');
    resetGame();
  }

  if (tankHealthMeter.value <= 0) {
    alert('Level 001 Completed! Prepare for the Crocodile Festival.');
    resetGame();
  }
}

function resetGame() {
  // Reset health meters
  carHealthMeter.value = 100;
  tankHealthMeter.value = 100;

  // Reset car and tank positions
  car.style.left = '0';
  car.style.top = '0';
  tank.style.left = '0';
  tank.style.top = '0';
}

// Call checkGameStatus after decreasing health
var tankHealthMeter = document.getElementById('tank-health');
function decreaseCarHealth() {
    
  carHealthMeter.value -= 10;
  checkGameStatus();
}

function decreaseTankHealth() {
    
  tankHealthMeter.value -= 10;
  checkGameStatus();
}
var ab=10000;
var ab = r
function low(){ 
 var r = 10000;
}
function med(){
 var r = 1000;
}
function hard(){
 var r = 10;
}
});
  </script>
</head>
<body>
  <meter id="car-health" min="0" max="100" value="100"></meter>
  <meter id="tank-health" min="0" max="100" value="100" style="color: blue;"></meter>
  <select style="position: relative;left:80px;">
  <option onclick="low()">Low</option>
  <option onclick="med()">Medium</option>
  <option onclick="hard()">Hard</option>
  </select>
  <div id="game-container">
    
    <img id="car" alt="car" src="https://nagasasank.files.wordpress.com/2024/01/download__17_-removebg-preview.png">
    
    <img id="tank" alt="tank" src="https://nagasasank.files.wordpress.com/2024/01/images__31_-removebg-preview.png">
  </div>
  <div style="display:grid;width:20px; position: relative;left:230px;">
  <button id="btn-up" style="background:url('https://nagasasank.files.wordpress.com/2024/01/download281729.jpeg'); background-size: contain;height:50px;width:50px;">
  </button>
  <p></p>
  <button id="btn-down" style="background:url('https://nagasasank.files.wordpress.com/2024/01/download281829.jpeg'); background-size:contain;height:50px;width:50px;">
  </button>
  </div>
  <div style="display:; position: relative;bottom:90px;left:200px;">
  <button id="btn-left" style="background:url('https://nagasasank.files.wordpress.com/2024/01/download282029.jpeg'); background-size:contain;height:50px;width:50px;">
  </button>
  <button id="btn-right" style="background:url('https://nagasasank.files.wordpress.com/2024/01/download281929.jpeg');background-size:contain;height:50px;width:50px;">
  </button>
  </div>
  <button id="btn-shoot-car"style="border-radius:20px;border:5px outset black;height:50px; position: relative;bottom:170px;background:url('https://nagasasank.files.wordpress.com/2024/01/images.png');background-size:contain;height:60px;width:60px;">
  ×
  </button>
  
</body>
</html>
        
