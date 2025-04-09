  I applied the demo code, and used your suggestions:
Code:

let colorlist = ['#e6194b', '#3cb44b', '#ffe119', '#4363d8', '#f58231', '#911eb4', '#46f0f0', '#f032e6', '#bcf60c', '#fabebe', '#008080', '#e6beff', '#9a6324', '#fffac8', '#800000', '#aaffc3', '#808000', '#ffd8b1', '#000075', '#808080', '#ffffff', '#000000'];

let bouncers = [];
let G = 0.1;
let wind = 0;

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < 10; i++) {
    bouncers.push(new Bouncer(random(width), random(height), 15, random(colorlist), random(-2, 2), random(-2, 2)));
  }
  ellipseMode(RADIUS);
}

function draw() {
  background(220);
  for (let b of bouncers) {
    b.applyGravity();
    b.applyWind();
    b.move();
  }
  checkCollisions();
  for (let b of bouncers) {
    b.draw();
  }
}

function keyPressed() {
  if (key === ' ') {
    wind = random(-0.5, 0.5);
  }
}

function checkCollisions() {
  for (let i = 0; i < bouncers.length; i++) {
    for (let j = i + 1; j < bouncers.length; j++) {
      let a = bouncers[i];
      let b = bouncers[j];
      let d = dist(a.x, a.y, b.x, b.y);
      if (d < a.r + b.r) {
        // simple elastic bounce by swapping velocities
        let tempDx = a.dx;
        let tempDy = a.dy;
        a.dx = b.dx;
        a.dy = b.dy;
        b.dx = tempDx;
        b.dy = tempDy;
      }
    }
  }
}

class Bouncer {
  constructor(x, y, r, c, dx, dy) {
    this.x = x;
    this.y = y;
    this.r = r;
    this.c = c;
    this.dx = dx;
    this.dy = dy;
    this.minX = r;
    this.maxX = width - r;
    this.minY = r;
    this.maxY = height - r;
  }

  applyWind() {
    this.dx += wind;
  }

  applyGravity() {
    this.dy += G;
  }

  move() {
    this.x += this.dx;
    this.y += this.dy;

    if (this.x < this.minX) {
      this.x = this.minX;
      this.dx *= -1;
    }
    if (this.x > this.maxX) {
      this.x = this.maxX;
      this.dx *= -1;
    }
    if (this.y < this.minY) {
      this.y = this.minY;
      this.dy *= -1;
    }
    if (this.y > this.maxY) {
      this.y = this.maxY;
      this.dy *= -1;
    }
  }

  draw() {
    fill(this.c);
    circle(this.x, this.y, this.r);
  }
}
