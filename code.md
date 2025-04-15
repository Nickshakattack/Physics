  I applied the demo code, and used your suggestions:
Code:


let colorlist = ['#e6194b', '#3cb44b', '#ffe119', '#4363d8', '#f58231', '#911eb4', '#46f0f0', '#f032e6', '#bcf60c', '#fabebe', '#008080', '#e6beff', '#9a6324', '#fffac8', '#800000', '#aaffc3', '#808000', '#ffd8b1', '#000075', '#808080', '#ffffff', '#000000'];

let bouncers = [];
let G = 0.1;
let wind = 0;

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < 10; i++) {
    bouncers.push(new Bouncer(random(width), random(height), 15, random(colorlist), random(0.5, 2)));
  }
  ellipseMode(RADIUS);
}

function draw() {
  background(220);
  for (let b of bouncers) {
    let gravity = createVector(0, G * b.mass);
    b.applyForce(gravity);

    let windForce = createVector(wind, 0);
    b.applyForce(windForce);

    b.update();
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
      let d = p5.Vector.dist(a.position, b.position);
      if (d < a.r + b.r) {
        // simple elastic bounce by swapping velocities
        let temp = a.velocity.copy();
        a.velocity = b.velocity.copy();
        b.velocity = temp;
      }
    }
  }
}

class Bouncer {
  constructor(x, y, r, c, mass) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-2, 2), random(-2, 2));
    this.acceleration = createVector(0, 0);
    this.r = r;
    this.c = c;
    this.mass = mass;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.checkEdges();
  }

  checkEdges() {
    if (this.position.x < this.r) {
      this.position.x = this.r;
      this.velocity.x *= -1;
    }
    if (this.position.x > width - this.r) {
      this.position.x = width - this.r;
      this.velocity.x *= -1;
    }
    if (this.position.y < this.r) {
      this.position.y = this.r;
      this.velocity.y *= -1;
    }
    if (this.position.y > height - this.r) {
      this.position.y = height - this.r;
      this.velocity.y *= -1;
    }
  }

  draw() {
    fill(this.c);
    circle(this.position.x, this.position.y, this.r);
  }
}


  draw() {
    fill(this.c);
    circle(this.x, this.y, this.r);
  }
}
