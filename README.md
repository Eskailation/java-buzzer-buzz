## Java buzzer buzz

> Simple use of the <a href="https://github.com/nyholku/purejavahidapi" target="_blank">PureJavaHidApi</a> to parse data of the buzzers from playstation buzz

---

## Setup

You need the PureJavaHidApi imported.
Then have the vendorID *0x054C* and productID *0x1000* ready.

## Dongle setup

Plugin the Buzz dongle and set into "connect" mode.

This can also be done with your own tools/code.

Im using <a href="http://janaxelson.com/hidpage.htm#tools">SimpleHIDWrite</a> before i run the code.

## SimpleHIDWrite setup

Start SimpleHIDWrite and select "WBuzz".

Fill the inputs with "00" (Every single of them).

Then click "Write".

Now you can turn on your buzzers and you can see the data when you push a button on the buzzer.

## Sample java code

```java
// get all devices
List<HidDeviceInfo> devList = PureJavaHidApi.enumerateDevices();
HidDeviceInfo devInfo = null;

// find the buzzer and break when found
for (HidDeviceInfo info : devList) {
    if (info.getVendorId() == (short) 0x054C && info.getProductId() == (short) 0x1000) {
        devInfo = info;
        break;
    }

}

// open buzzer device
final HidDevice dev = PureJavaHidApi.openDevice(devInfo);

// set input listener to receive the data
dev.setInputReportListener((HidDevice source, byte Id, byte[] data, int len) -> {
    String buzzerData = Arrays.toString(data);
    Platform.runLater(() -> {
        try {
          // parse the data here (buzzerData)
        } catch (InterruptedException ex) {
          // catch your errors
        }
    });
});
```

---

## Possible buzzer data

The data might be different with your buzzers.

```java
/*  Team 1  */
public static String Team1Buzzer = "[0, 0, 32, 0, -16]";
public static String Team1Blue = "[0, 0, 0, 2, -16]";
public static String Team1Orange = "[0, 0, 0, 1, -16]";
public static String Team1Green = "[0, 0, -128, 0, -16]";
public static String Team1Yellow = "[0, 0, 64, 0, -16]";

/*  Team 2  */
public static String Team2Buzzer = "[0, 0, 1, 0, -16]";
public static String Team2Blue = "[0, 0, 16, 0, -16]";
public static String Team2Orange = "[0, 0, 8, 0, -16]";
public static String Team2Green = "[0, 0, 4, 0, -16]";
public static String Team2Yellow = "[0, 0, 2, 0, -16]";

/*  Team 3  */
public static String Team3Buzzer = "[0, 0, 0, 4, -16]";
public static String Team3Blue = "[0, 0, 0, 64, -16]";
public static String Team3Orange = "[0, 0, 0, 32, -16]";
public static String Team3Green = "[0, 0, 0, 16, -16]";
public static String Team3Yellow = "[0, 0, 0, 8, -16]";

/*  Team 4  */
public static String Team4Buzzer = "[0, 0, 0, -128, -16]";
public static String Team4Blue = "[0, 0, 0, 0, -8]";
public static String Team4Orange = "[0, 0, 0, 0, -12]";
public static String Team4Green = "[0, 0, 0, 0, -14]";
public static String Team4Yellow = "[0, 0, 0, 0, -15]";
```

You have different buzzerData when two buttons are pressed the exact same time.
```java
public static String Team1And2 = "[0, 0, 33, 0, -16]";
public static String Team1And3 = "[0, 0, 32, 4, -16]";
public static String Team1And4 = "[0, 0, 32, -128, -16]";
public static String Team2And3 = "[0, 0, 1, 4, -16]";
public static String Team2And4 = "[0, 0, 1, -128, -16]";
public static String Team3And4 = "[0, 0, 8, -124, -16]";
```

Its useful to debug/print all the possible buzzerData you'll need.
