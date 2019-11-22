# mouse-sound-interaction

    import ddf.minim.*;
    Minim minim;
    AudioPlayer[] sound = new AudioPlayer[8];
    import spout.*;

    ArrayList<Particle> particles; 
    Spout spout;

    void setup() {
      size(640, 360, P2D);

      minim = new Minim(this);
      sound[0] = minim.loadFile("do.wav");
      sound[1] = minim.loadFile("re.wav");
      sound[2] = minim.loadFile("mi.wav");
      sound[3] = minim.loadFile("fa.wav");
      sound[4] = minim.loadFile("sol.wav");
      sound[5] = minim.loadFile("la.wav");
      sound[6] = minim.loadFile("do menor.wav");
      sound[7] = minim.loadFile("si.wav");

      spout = new Spout(this);
      spout.createSender("Spout Processing");

      particles = new ArrayList<Particle>();
      for (int i = 0; i < 8; i++)
        particles.add(new Particle(150+100*(i%4), height/2-50+100*(floor(i/4)), 50, i ) );
    }

    void draw() {
      background(0,0,0);
      for (Particle p : particles)  p.display();

      spout.sendTexture();
    }

    class Particle {
      int x, y, diameter, idx;
      boolean onlyonce=true;

      Particle (int _x, int _y, int _diameter, int _idx) {
        x = _x;
        y = _y;
        diameter = _diameter;
        idx=_idx;
      }

      boolean over() {
        return ( dist(mouseX, mouseY, x, y) < diameter/2 );
      }

      void play(int i ) { //this is gold
      if ( ! sound[i].isPlaying() ) {
        sound[i].rewind();
        sound[i].play();
        println("start play ", i);
      }
    }

      void display() {
        stroke(250);
        strokeWeight(1);
        noFill();
        if ( over() ) { // si esto es cierto, entonces
          strokeWeight(5);
          fill(x,y,150);
          if ( onlyonce ) play(idx); //___________________ call global sound handler. Si dentro de lo primero, esto es cierto
          onlyonce = false;
        } else {
          onlyonce = true; //_ only first time
        }    
        ellipse(x, y, diameter, diameter);
      }
    }
