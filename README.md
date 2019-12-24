## YAZILIM MİMARİSİ ÖDEVİ
### ADAPTER PATTENT
Sistemdeki soyutlamaları sağlamak için interface’ler abstract sınıflar kullanıldığından bahsetmiştik. Bu desende mevcut bulunan yapıya uymayan bir sınıfı entegre etme tekniğini inceliyeceğiz. Adapter Deseni, sisteme yeni bir özelliğin eklenmesini kolaylaştıran bir tekniktir. Adapter Desenininde, bir adaptör sınıfı yazılır ve bu sınıftan adapte edilmeye çalışılan sınıfa bir referans vardır. Bu sayede yazılmış olan adaptör sınıfı mevcut yapıya entegre bir şekilde çalışır. Aslında bu yapılan iş arka tarafta sisteme yeni bir arayüz kazandırmaktan başka bir şey değildir. Bu sebeple facade tasarım deseni ile ortak noktaları vardır. Ancak, birinin amacı sistemi basitleştirmek, kod kalabalığını engellemek iken, adaptörün asıl hedefi sisteme yeni bileşenleri entegre edebilmektir.

![Image of Class](https://github.com/busekren/Yaz-l-mMimarisi-dev/blob/master/Adapter_diagram.png.jpg)

''' AdapterPatternDemo.java

      public class AdapterPatternDemo {
       public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();
        audioPlayer.play("mp3", "beyond the horizon.mp3");
        audioPlayer.play("mp4", "alone.mp4");
        audioPlayer.play("vlc", "far far away.vlc");
        audioPlayer.play("avi", "mind me.avi");
    }
}

'''AdvancedMediaPlayer.java

     public class AdvancedMediaPlayer {

    public void playVlc(String fileName) {

    }

    public void playMp4(String fileName) {

    }
}

'''AudioPlayer.java

public class AudioPlayer extends MediaPlayer {
    MediaAdapter mediaAdapter;

    @Override
    public void play(String audioType, String fileName) {

        //inbuilt support to play mp3 music files
        if(audioType.equalsIgnoreCase("mp3")){
            System.out.println("Playing mp3 file. Name: " + fileName);
        }

        //mediaAdapter is providing support to play other file formats
        else if(audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")){
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        }

        else{
            System.out.println("Invalid media. " + audioType + " format not supported");
        }
    }
}
'''MediaAdapter.java
     
     public class MediaAdapter extends MediaPlayer {

    AdvancedMediaPlayer advancedMusicPlayer;

    public MediaAdapter(String audioType){

        if(audioType.equalsIgnoreCase("vlc") ){
            advancedMusicPlayer = new VlcPlayer();

        }else if (audioType.equalsIgnoreCase("mp4")){
            advancedMusicPlayer = new Mp4Player();
        }
    }

    @Override
    public void play(String audioType, String fileName) {

        if(audioType.equalsIgnoreCase("vlc")){
            advancedMusicPlayer.playVlc(fileName);
        }
        else if(audioType.equalsIgnoreCase("mp4")){
            advancedMusicPlayer.playMp4(fileName);
        }
    }
}
'''MediaPlayer.java
      
      public class MediaPlayer {
       public void play(String audioType, String fileName) {

    }
}

'''Mp4Player.java
    
    public class Mp4Player extends AdvancedMediaPlayer {

    @Override
    public void playVlc(String fileName) {
        //do nothing
    }

    @Override
    public void playMp4(String fileName) {
        System.out.println("Playing mp4 file. Name: "+ fileName);
    }
}

'''VlcPlayer.java

    public class VlcPlayer extends AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        System.out.println("Playing vlc file. Name: "+ fileName);
    }

    @Override
    public void playMp4(String fileName) {
        //do nothing
    }
}

### VİSİTOR PETTERN 
Ziyaretçi tasarım deseni, çok sayıda ve farklı tipteki nesneler üzerinde işlem yapabilmek amacıyla kullanılır. İşlem yapılacak nesnelerde herhangi bir değişiklik yapılmaz. İşlemi ziyaretçi nesneleri yapar. Eğer sisteme yeni nesneler eklenmiyor, fakat sık sık yeni işlemlerin eklenmesi gerekiyorsa bu tasarım deseni kullanılabilir. Bu tasarım deseninin kullanılmasıyla, yapılacak işlemle ilgili kodların merkezi bir nesnede toplanır.
![Image of Class](https://github.com/busekren/Yaz-l-mMimarisi-dev/blob/master/Visitor_diagram.png.jpg)

'''Computer.java
   
     public class Computer extends ComputerPart {

    ComputerPart[] parts;

    public Computer(){
        parts = new ComputerPart[] {new Mouse(), new Keyboard(), new Monitor()};
    }


    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        for (int i = 0; i < parts.length; i++) {
            parts[i].accept(computerPartVisitor);
        }
        computerPartVisitor.visit(this);
    }
}
'''ComputerPart.java

    public class ComputerPart {
    public void accept(ComputerPartVisitor computerPartVisitor) {

    }
    }
    
 '''ComputerPartDisplayVisitor.java
 
     public class ComputerPartDisplayVisitor implements ComputerPartVisitor {

    @Override
    public void visit(Computer computer) {
        System.out.println("Displaying Computer.");
    }

    @Override
    public void visit(Mouse mouse) {
        System.out.println("Displaying Mouse.");
    }

    @Override
    public void visit(Keyboard keyboard) {
        System.out.println("Displaying Keyboard.");
    }

    @Override
    public void visit(Monitor monitor) {
        System.out.println("Displaying Monitor.");
    }
}
'''ComputerPartVisitor.java

    public interface ComputerPartVisitor {
    public void visit(Computer computer);
    public void visit(Mouse mouse);
    public void visit(Keyboard keyboard);
    public void visit(Monitor monitor);
    }
'''Keyboard.java

public class Keyboard extends ComputerPart {

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
    }
    
 '''Monitor.java
 public class Monitor extends ComputerPart {

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
    }
 '''Mouse.java
 public class Mouse extends ComputerPart {

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
    }
''' VisitorPatternDemo.java
     
     public class VisitorPatternDemo {
    public static void main(String[] args) {

        ComputerPart computer = new Computer();
        computer.accept(new ComputerPartDisplayVisitor());
    }
    }
 
