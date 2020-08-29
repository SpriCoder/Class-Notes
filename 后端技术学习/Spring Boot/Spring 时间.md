Spring 时间
---
1. 主要涉及到LocalDateTime和TimeStamp之间的转换

```java
@Component
public class TimeUtil {
    public static LocalDateTime getLocalDateTime(long timestamp){
        LocalDateTime time = LocalDateTime.ofEpochSecond(timestamp/1000,0, ZoneOffset.ofHours(0));
        return time;
    }

    public static LocalDateTime getDefaultLocalDateTime(){
        LocalDateTime time = LocalDateTime.ofEpochSecond(946656000/1000,0,ZoneOffset.ofHours(0));
        return time;
    }

    //毫秒为单位
    public static long getTimeStamp(LocalDateTime localDateTime){
        return localDateTime.toInstant(ZoneOffset.of("+8")).toEpochMilli();
    }

    public static String dateToString(LocalDateTime localDateTime){
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        String dateTime = localDateTime.format(formatter);
        return dateTime;
    }

    public static String dateToAreaString(LocalDateTime localDateTime){
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MM月dd日 HH:mm");
        String dateTime = localDateTime.format(formatter);
        return dateTime;
    }

    public static String dateToVOString(LocalDateTime localDateTime){
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
        String dateTime = localDateTime.format(formatter);
        return dateTime;
    }

    public static String preTime(){
        return dateToVOString(LocalDateTime.now()) + " ";
    }

    public static LocalDateTime toDay(LocalDateTime input){
        String time = dateToString(input);
        String[] temp = time.split("-");
        return LocalDateTime.of(Integer.valueOf(temp[0]),Integer.valueOf(temp[1]), Integer.valueOf(temp[2]),0,0);
    }

    /**
     * 获取本周一
     * @return
     */
    public static LocalDateTime weekFirstDay(){
        LocalDateTime localDateTime = LocalDateTime.now();
        int days_of_week = localDateTime.getDayOfWeek().getValue();
        localDateTime = localDateTime.minusDays(days_of_week - 1);
        return toDay(localDateTime);
    }
}
```