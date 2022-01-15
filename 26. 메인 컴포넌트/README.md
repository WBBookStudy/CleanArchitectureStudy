# 26. 메인 컴포넌트
모든 시스템에는 최소한 하나의 컴포넌트가 존재하고, 이 컴포넌트가 나머지 컴포넌트를 생성하고, 조정하며, 관리한다. 이 컴포넌트가 메인컴포넌트이다.

## 궁극적인 세부사항
메인 컴포넌트는 궁극적인 세부사항으로, 가장 낮은 수준의 정책이다.  
메인은 모든 팩토리(Factory)와 전략(Stategy), 그리고 시스템 전반을 담당하는 나머지 기반 설비를 생성한 후, 시스템에서 더 높은 수준을 담당하는 부분으로 제어권을 넘기는 역할을 맡는다.  
의존성을 주입하는 일은 이 메인 컴포넌트에서 이루어져야한다.  
```Java
public class Main implements HtwMessageReceiver {
  private static HuntTheWumpus game;
  private static int hitPoints = 10;
  private static final List<String> caverns = new ArrayList<>();
  // 코드의 나머지 핵심 영역에서 구체적인 문자열을 알지 못하게 함
  private static final String[] environments = new String[]{
    "bright",
    "humid",
    "dry",
    "creepy",
    "ugly",
    "foggy",
    "hot",
    "cold",
    "drafty",
    "dreadful"
  };
  
   private static final String[] shapes = new String[] {
    "round",
    "square",
    "oval",
    "irregular",
    "long",
    "craggy",
    "rough",
    "tall",
    "narrow"
  };

  ...

public static void main(String[] args) throws IOException {
  //게임 만듬
    game = HtwFactory.makeGame("htw.game.HuntTheWumpusFacade", new Main());
  // 맵 생성
    createMap();
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    game.makeRestCommand().execute();
    while (true) {
      System.out.println(game.getPlayerCavern());
      System.out.println("Health: " + hitPoints + " arrows: " + game.getQuiver());
      HuntTheWumpus.Command c = game.makeRestCommand();
      System.out.println(">");
      String command = br.readLine();
      if (command.equalsIgnoreCase("e"))
        // 명령어를 실제로 처리하는 일은 고수준 컴포넌트로 위임
        c = game.makeMoveCommand(EAST);
      else if (command.equalsIgnoreCase("w"))
        c = game.makeMoveCommand(WEST);
      else if (command.equalsIgnoreCase("n"))
        c = game.makeMoveCommand(NORTH);
      else if (command.equalsIgnoreCase("s"))
        c = game.makeMoveCommand(SOUTH);
      else if (command.equalsIgnoreCase("r"))
        c = game.makeRestCommand();
      else if (command.equalsIgnoreCase("sw"))
        c = game.makeShootCommand(WEST);
      else if (command.equalsIgnoreCase("se"))
        c = game.makeShootCommand(EAST);
      else if (command.equalsIgnoreCase("sn"))
        c = game.makeShootCommand(NORTH);
      else if (command.equalsIgnoreCase("ss"))
        c = game.makeShootCommand(SOUTH);
      else if (command.equalsIgnoreCase("q"))
        return;

      c.execute();
    }
  }
}
```
> 요지는 메인 클린 아키텍처에서 가장 바깥 원에 위치하는, 지저분한 저수준 모듈이라는 점이다.















