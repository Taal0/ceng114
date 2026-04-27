# 06 — Event-Driven Programming (JavaFX)

> **Mantık:** Klasik program **yukarıdan aşağıya** çalışır. Event-driven program **olaylar olduğunda** kod çalıştırır (butona tıklama, klavye, fare, vs.). JavaFX bunun için **handler**'lar kullanır.

---

## Üç Temel Kavram

| Kavram | Ne demek? | Örnek |
|---|---|---|
| **Event Source** | Olayı üreten bileşen | `Button`, `TextField` |
| **Event** | Olayın kendisi | `ActionEvent`, `MouseEvent` |
| **Event Handler** | Olay olduğunda çalışacak kod | `EventHandler<T>` |

---

## En Temel JavaFX Programı

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.stage.Stage;
import javafx.scene.layout.StackPane;

public class HelloFX extends Application {
    @Override
    public void start(Stage primaryStage) {
        Button btn = new Button("Tıkla");

        // EVENT HANDLER
        btn.setOnAction(e -> System.out.println("Butona basıldı"));

        StackPane root = new StackPane(btn);
        primaryStage.setScene(new Scene(root, 300, 200));
        primaryStage.setTitle("Demo");
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## Handler Yazmanın 3 Yolu

### 1) Lambda (en yaygın, EN KISA)

```java
btn.setOnAction(e -> System.out.println("Tıklandı"));

btn.setOnAction(e -> {
    label.setText("Merhaba");
    counter++;
});
```

### 2) Anonymous inner class

```java
btn.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent e) {
        System.out.println("Tıklandı");
    }
});
```

### 3) Ayrı sınıf (büyük projeler)

```java
class MyHandler implements EventHandler<ActionEvent> {
    @Override
    public void handle(ActionEvent e) {
        System.out.println("Tıklandı");
    }
}

// kullanım
btn.setOnAction(new MyHandler());
```

> Quiz'de **lambda** versiyonu yeter. Hoca özellikle "anonymous class kullan" demediyse lambda yaz.

---

## Sık Kullanılan Event'ler

| Source | Event method | Event tipi |
|---|---|---|
| `Button` | `setOnAction` | `ActionEvent` |
| `TextField` | `setOnAction` (Enter'a basınca) | `ActionEvent` |
| Herhangi bir Node | `setOnMouseClicked` | `MouseEvent` |
| Herhangi bir Node | `setOnMousePressed` | `MouseEvent` |
| Scene | `setOnKeyPressed` | `KeyEvent` |
| Scene | `setOnKeyTyped` | `KeyEvent` |

---

## Tam Örnek: Sayaç (Counter)

```java
import javafx.application.Application;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class CounterApp extends Application {
    private int count = 0;

    @Override
    public void start(Stage stage) {
        Label label = new Label("0");
        Button plus  = new Button("+");
        Button minus = new Button("-");

        plus.setOnAction(e -> {
            count++;
            label.setText(String.valueOf(count));
        });

        minus.setOnAction(e -> {
            count--;
            label.setText(String.valueOf(count));
        });

        HBox buttons = new HBox(10, minus, plus);
        buttons.setAlignment(Pos.CENTER);

        VBox root = new VBox(20, label, buttons);
        root.setAlignment(Pos.CENTER);

        stage.setScene(new Scene(root, 250, 150));
        stage.setTitle("Sayaç");
        stage.show();
    }

    public static void main(String[] args) { launch(args); }
}
```

**Burada dikkat:**
- `count` field olarak (instance variable) tanımlandı çünkü lambda **dış değişkeni effectively final** olarak görür. Local `int count` lambda içinden değiştirilemez.

---

## Tam Örnek: Mouse Olayı

```java
Circle circle = new Circle(50);
circle.setFill(Color.BLUE);

circle.setOnMouseClicked(e -> {
    if (e.getButton() == MouseButton.PRIMARY) {
        circle.setFill(Color.RED);
    } else {
        circle.setFill(Color.GREEN);
    }
});
```

---

## Tam Örnek: Klavye Olayı

```java
Scene scene = new Scene(root, 400, 300);
scene.setOnKeyPressed(e -> {
    switch (e.getCode()) {
        case UP:    label.setText("Yukarı"); break;
        case DOWN:  label.setText("Aşağı");  break;
        case LEFT:  label.setText("Sol");    break;
        case RIGHT: label.setText("Sağ");    break;
    }
});
```

---

## Layout Hızlı Bakış

| Layout | Ne yapar? |
|---|---|
| `HBox` | Yatay sıralama |
| `VBox` | Dikey sıralama |
| `GridPane` | Grid (satır, sütun) |
| `BorderPane` | Top, Bottom, Left, Right, Center |
| `StackPane` | Üst üste yerleştirme |
| `FlowPane` | Akan yerleşim (sığmazsa alta) |

---

## Sık Yapılan Hatalar

1. **`launch(args)` çağırmamak** → uygulama açılmaz.
2. **`extends Application` unutmak.**
3. **`primaryStage.show()` çağırmamak** → pencere görünmez.
4. **Lambda içinde local değişken değiştirmek** → "effectively final" hatası. Field yap.
5. **Yanlış event metodu** — `Button.setOnMouseClicked` yerine `setOnAction` (ikisi de çalışır ama action daha doğru semantik).
6. **Handler'ı `setOnAction` yerine `addEventHandler` ile karıştırmak.**

---

## Quiz'de Sorulabilecek

1. "**Bir butona tıklayınca** label'daki sayıyı 1 artıran program yaz."
2. "**İki textfield + buton**: butona basınca toplamlarını label'a yaz."
3. "**Klavyede ok tuşlarına basınca** bir circle'ı hareket ettir."
4. "**Mouse'a tıklayınca** o noktada bir circle çiz."
5. "**Buton + radio button**: hangi radio seçiliyse butona göre farklı şey yap."

### Quiz şablonu (toplama uygulaması):

```java
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class AdderApp extends Application {
    @Override
    public void start(Stage stage) {
        TextField tfA = new TextField();
        TextField tfB = new TextField();
        TextField tfResult = new TextField();
        tfResult.setEditable(false);
        Button btn = new Button("Topla");

        btn.setOnAction(e -> {
            try {
                double a = Double.parseDouble(tfA.getText());
                double b = Double.parseDouble(tfB.getText());
                tfResult.setText(String.valueOf(a + b));
            } catch (NumberFormatException ex) {
                tfResult.setText("Hata!");
            }
        });

        GridPane grid = new GridPane();
        grid.setHgap(10); grid.setVgap(10); grid.setPadding(new Insets(20));
        grid.addRow(0, new Label("A:"), tfA);
        grid.addRow(1, new Label("B:"), tfB);
        grid.addRow(2, btn, tfResult);

        stage.setScene(new Scene(grid, 300, 200));
        stage.setTitle("Topla");
        stage.show();
    }

    public static void main(String[] args) { launch(args); }
}
```
