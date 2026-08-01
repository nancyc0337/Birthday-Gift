<!DOCTYPE html>
<html>
<head>
  <!-- Required meta tags -->
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Iansui&display=swap" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" />
  <style>
    /* CSS */
    html, body {
      height: 100%;
      margin: 0;
    }
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #f0f0f0;
    }
  </style>
  <title>Happy Birthday</title>
</head>
<body>
  <!-- HTML -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.js"></script>
  <script>
    // JS
    let birdieHit = false;
    let birdieY = 310;
    let birdieX = 340;
    let currentLanguage = 'none'; // Tracks which language to display

    function setup() {
      let cnv = createCanvas(1400, 600);
      cnv.elt.style.border = '10px solid darkgray';
      cnv.elt.style.borderRadius = '20px';
    }

    function draw() {
      background(240); // Light gray background

      // --- START OF SCALED RAT BLOCK ---
      push();
      // 1. Move the drawing origin so the enlarged rat stays centered and touches the ground
      translate(120, 112);
      // 2. Scale everything inside this push/pop block by 1.5x
      scale(1.5);

      // Static center coordinates for the rat relative to the translation matrix
      let x = 90;
      let y = 229;

      // --- TAIL ---
      stroke(220, 160, 160); // Pinkish tail
      strokeWeight(4);
      noFill();
      bezier(x - 50, y + 40, x - 90, y + 60, x - 80, y + 20, x - 100, y + 30);

      // --- BODY ---
      noStroke();
      fill(130); // Gray fur
      ellipse(x, y + 20, 110, 80); // Main body

      // --- HEAD ---
      ellipse(x + 50, y - 10, 70, 50); // Head base

      // --- SNOUT/NOSE ---
      fill(240, 180, 180); // Pink nose
      ellipse(x + 85, y - 10, 12, 12);

      // --- EARS ---
      // Outer Ear (Gray)
      fill(130);
      ellipse(x + 35, y - 35, 30, 35);
      // Inner Ear (Pink)
      fill(240, 180, 180);
      ellipse(x + 35, y - 35, 20, 25);

      // --- EYE ---
      fill(0); // Black eye
      ellipse(x + 65, y - 15, 6, 6);

      // --- BACK PAWS / FEET ---
      fill(240, 180, 180);
      ellipse(x - 30, y + 58, 25, 12); // Left back foot
      ellipse(x + 20, y + 58, 25, 12); // Right back foot

      // --- FRONT ARM (HOLDING RACKET) ---
      fill(110); // Slightly darker gray for depth
      ellipse(x + 45, y + 25, 25, 15); // Hand/arm reaching forward

      // --- BADMINTON RACKET ---
      push();
      translate(x + 50, y + 25); // Move origin to the rat's hand
      rotate(radians(-30)); // Tilt the racket upward

      // 1. Racket Shaft/Handle
      stroke(50); // Dark gray metallic shaft
      strokeWeight(3);
      line(0, 30, 0, -60); // Handle extends down, shaft extends up

      // 2. Grip Tape
      stroke(255, 215, 0); // Gold/yellow grip tape
      strokeWeight(5);
      line(0, 15, 0, 35);

      // 3. Racket Head (Frame)
      noFill();
      stroke(200, 50, 50); // Red frame
      strokeWeight(4);
      ellipse(0, -95, 50, 70); // Oval frame

      // 4. Racket Strings (Grid)
      stroke(255, 150); // Semi-transparent white strings
      strokeWeight(1);
      // Vertical strings
      line(-15, -115, -15, -75);
      line(-7, -125, -7, -65);
      line(0, -130, 0, -60);
      line(7, -125, 7, -65);
      line(15, -115, 15, -75);
      // Horizontal strings
      line(-18, -110, 18, -110);
      line(-23, -100, 23, -100);
      line(-25, -90, 25, -90);
      line(-23, -80, 23, -80);
      line(-18, -70, 18, -70);
      pop();

      pop();
      // --- END OF SCALED RAT BLOCK ---

      // --- COMPLETED BIRDIE ---
      push();
      // If clicked, make the birdie fly up and away!
      if (birdieHit) {
        birdieX += 8;
        birdieY -= 6;
      }
      translate(birdieX, birdieY);
      rotate(radians(-45)); // Angled flight path orientation

      // 1. Feathers / Skirt (Lines & Transparent Shape)
      fill(255, 255, 255, 180);
      stroke(180);
      strokeWeight(1.5);
      quad(-6, -10, 6, -10, 18, -35, -18, -35); // Outer mesh fan

      // Internal feather ribs
      line(-3, -10, -10, -35);
      line(0, -10, 0, -35);
      line(3, -10, 10, -35);

      // 2. Cork Base (White Half-Dome)
      fill(255);
      noStroke();
      arc(0, -5, 16, 16, 0, PI, CHORD);

      // 3. Nose Ribbon (Red Base Coating)
      fill(200, 50, 50);
      rectMode(CENTER);
      rect(0, -7, 16, 4);
      pop();

      // ground
      fill("#8AFF97");
      noStroke();
      rect(0, 550, 1400, 50);

      textFont('Iansui');
      textStyle(BOLD);
      fill("#000000");
      textSize(32);

      // Dynamic instructional text changes after clicking
      if(!birdieHit) {
        text("Click the birdie.", 550, 585);
      } else {
        text("Press 'E' or 'C' or 'S'.", 500, 585);
      }

      // Title header logic
      if (birdieHit && currentLanguage === 'english') {
        fill("#000000");
        textSize(50);
        text("🎉Happy Birthday!🎉", 475, 100);
      } else if (birdieHit && currentLanguage === 'chinese') {
        fill("#000000");
        textSize(50);
        text("🎉生日快乐!🎉", 550, 100);
      } else if (birdieHit && currentLanguage === 'spanish') {
        fill("#000000");
        textSize(50);
        text("🎉¡Feliz cumpleaños!🎉", 425, 100);
      }
    }

    // Handles the clicking interaction requested in your prompt text
    function mousePressed() {
      let d = dist(mouseX, mouseY, birdieX, birdieY);
      // Proximity check around the birdie coordinates
      if (d < 40) {
        birdieHit = true;
      }
    }

    // STANDALONE FUNCTION: Must live outside the draw loop
    function keyPressed() {
      if (key === 'e' || key === 'E') {
        currentLanguage = 'english';
      } else if (key === 'c' || key === 'C') {
        currentLanguage = 'chinese';
      } else if (key === 's' || key === 'S') {
        currentLanguage = 'spanish';
      }
    }
  </script>
</body>
</html>

