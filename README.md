# mouse-sound-interaction
    import ddf.minim.*;
    Minim minim;
    AudioPlayer sound1;

    ArrayList<Particle> particles; //gonna have arraylist. full of Particle objects. named particles
    boolean playFile = false;

    void setup() {
     size(640,360);

     minim = new Minim(this);
     sound1 = minim.loadFile("do.wav");

     particles = new ArrayList<Particle>();
     for (int i = 0; i < 4; i++) {
       particles.add(new Particle(100*i+100,height/2,50)); 
       particles.add(new Particle(100*i+100,height/2-100,50)); 
       }
    }

    void draw() {
      background(200);

      //Particle p = particles.get(0); //Con un solo objeto de la clase, la interaccion funciona
      //p.update(mouseX, mouseY);
      //p.display();

      for (Particle p : particles) {
        p.display();
        p.update(mouseX, mouseY);
      }
    }

    void mousePressed() {
    if (playFile) {
        sound1.play();
        sound1.rewind();
      }
    }
    
Mi clase / my class

    class Particle {
      int x;
      int y;
      int diameter;
      int relleno;

      Particle (int tempX, int tempY, int tempR) {
        x = tempX;
        y = tempY;
        diameter = tempR;
      }

      boolean overCircle(int x, int y, int diameter) {
        if (dist(mouseX, mouseY, x, y) < diameter/2) {
          return true;
        } else {
          return false;
        }
      }

    void update(int mouseXin, int mouseYin) {
     if (overCircle( x, y, diameter)) {
       playFile = true;
     } else {
       playFile = false;
     }
    }

      void display() {
        noStroke();
        if (dist(mouseX, mouseY, x, y) < diameter/2) {
          fill(x,y,150);
          sound1.play();
        } else {
          stroke(1);
          noFill();
       }
        ellipse(x,y,diameter,diameter); 
      }  
    }
