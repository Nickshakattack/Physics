Assignment: 
Interactivty lab:
  Continue to work in your Physics repository. 
Add interactivity to your program. In class, I demonstrated using keyboard presses to add forces to a Mover. As you know, however, you have many options for including interactivty in your
programs, 
including mouse movement/clicks, touches, and even the accelerometer! It helps, of ccourse, to have a vision of what you want to achieve and puzzles and games are somewhat useful here 
(flappy bird like movement, platformers, a marble in labyrinth, etc.)
Feel free also to include other forces (like the liquid and friction examples in Chapter 2 of The Nature of Code) to extend your ideas. 
Resubmit the link to your repository here. 

New, Updated Code:

let colorlist = [
  '#e6194b', '#3cb44b', '#ffe119', '#4363d8', '#f58231',
  '#911eb4', '#46f0f0', '#f032e6', '#bcf60c', '#fabebe',
  '#008080', '#e6beff', '#9a6324', '#fffac8', '#800000',
  '#aaffc3', '#808000', '#ffd8b1', '#000075', '#808080',
  '#ffffff', '#000000'
];

let bouncers = [];
let G = 0.1;
let wind = 0;

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < 10; i++) {
    bouncers.push(
      new Bouncer(
        random(width),
        random(height),
        15,
        random(colorlist),
        random(0.5, 2)
      )
    );
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

    if (keyIsDown(LEFT_ARROW)) {
      b.applyForce(createVector(-0.2, 0));
    }
    if (keyIsDown(RIGHT_ARROW)) {
      b.applyForce(createVector(0.2, 0));
    }
    if (keyIsDown(UP_ARROW)) {
      b.applyForce(createVector(0, -0.3));
    }
    if (keyIsDown(DOWN_ARROW)) {
      b.applyForce(createVector(0, 0.2));
    }

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

function mousePressed() {
  for (let b of bouncers) {
    b.applyForce(createVector(0, -3));
  }
}

function checkCollisions() {
  for (let i = 0; i < bouncers.length; i++) {
    for (let j = i + 1; j < bouncers.length; j++) {
      let a = bouncers[i];
      let b = bouncers[j];
      let d = p5.Vector.dist(a.position, b.position);
      if (d < a.r + b.r) {
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
