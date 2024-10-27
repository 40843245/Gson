# `com.google.gson.Gson`
## usage
### serialization

To serialize a Java class to JSON data. One can call `toJson` method of `Gson` instance.

For example, 

To serialize a `DietDiary object to JSON data. One can use the following code snippets. 

```
import com.google.gson.Gson;
import com.google.gson.JsonObject;

Gson gson = new Gson();
JsonObject jsonObject = new JsonObject();
DietDiary dietDiary = new DietDiary();

dietDiary.setDiaryId(2);
dietDiary.setUserId(5);

jsonObject = gson.toJson(dietDiary);
```

where `DietDiary` class is defined as follows.

```
public class DietDiary {
	private int diaryId;
	private int userId;
	private Date createDate;
	private Time createTime;
	
	public int getDiaryId() {
		return diaryId;
	}
	public void setDiaryId(int diaryId) {
		this.diaryId = diaryId;
	}
	public int getUserId() {
		return userId;
	}
	public void setUserId(int userId) {
		this.userId = userId;
	}
}
```

## deserialization
To deserialize JSON data to a Java object, one can call `fromJson` method to `gson` instance.

Take above example.

To deserialize JSON data to `Member` object. Look at following example.

```
import com.google.gson.Gson;
import com.google.gson.JsonObject;

Gson gson = new Gson();
JsonObject jsonObject = new JsonObject();
DietDiary dietDiary = new DietDiary();

dietDiary.setDiaryId(2);
dietDiary.setUserId(5);

jsonObject = gson.toJson(dietDiary);

dietDiary = gson.fromJson(jsonObject,DietDiary.class);
```

where `DietDiary` class is defined as above section.

## use your own settings for Gson

To use your own settings for Gson, create a `GsonBuilder` instance, setting it through invoking its method, lastly, create a `Gson` instance by calling `create` method with the `GsonBuilder` instance.

Consider the following code snippets.

```
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;

GsonBuilder  = new GsonBuilder();
gsonBuilder.setDateFormat("yyyy-MM-dd HH:mm:ss");
Gson gson = gsonBuilder.create()
```

## customize your serialization
To customize your serialization, follow these steps.

1. define a class that implements `JsonSerializer` interface and implement its all unimplemented method -- `serialize` method.

The `serialize` method in `JsonSerializer` interface determines how Gson serialize the Java Object.

2. Register the class defined in step 1 through invoking `registerTypeAdapter` method in `GsonBuilder` instance.

3. Create the `Gson` instance by invoking `create` method in `GsonBuilder` instance.

Consider the following example.

```
public class Id<T> {
   private final Class<T> clazz;
   private final long value;

   public Id(Class<T> clazz, long value) {
     this.clazz = clazz;
     this.value = value;
   }

   public long getValue() {
     return value;
   }
 }
```

```
class IdSerializer implements JsonSerializer<Id> {
   public JsonElement serialize(Id id, Type typeOfId, JsonSerializationContext context) {
     return new JsonPrimitive(id.getValue());
   }
 }
```

```
Gson gson = new GsonBuilder().registerTypeAdapter(Id.class, new IdSerializer()).create();
```

For more details, see [API about `JsonSerializer`](https://www.javadoc.io/doc/com.google.code.gson/gson/latest/com.google.gson/com/google/gson/JsonSerializer.html)

## customize your deserialization
To customize your deserialization, follow these steps.

1. define a class that implements `JsonDeserializer` interface and implement its all unimplemented method -- `deserialize` method.

The `deserialize` method in `JsonDeserializer` interface determines how Gson serialize the Java Object.

2. Register the class defined in step 1 through invoking `registerTypeAdapter` method in `GsonBuilder` instance.

3. Create the `Gson` instance by invoking `create` method in `GsonBuilder` instance.

Consider the following code snippets.

```
import java.sql.Date;

// ...

 JsonDeserializer<Date> dateDeserializer = new JsonDeserializer<Date>() {
  @Override
  public Date deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) throws JsonParseException {
   SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd");
   try {
    return new Date(dateFormat.parse(json.getAsString()).getTime());
   } catch (ParseException e) {
    throw new JsonParseException(e);
   }
  }
 };
```

```
import java.sql.Time;

// ...

 JsonDeserializer<Time> timeDeserializer = new JsonDeserializer<Time>() {
  @Override
  public Time deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) throws JsonParseException {
   SimpleDateFormat timeFormat = new SimpleDateFormat("HH:mm:ss");
   try {
    return new Time(timeFormat.parse(json.getAsString()).getTime());
   } catch (ParseException e) {
    throw new JsonParseException(e);
   }
  }
 };
```

```
import java.sql.Date;
import java.sql.Time;

// ...

 Gson gson = new GsonBuilder()
  .registerTypeAdapter(Date.class, dateDeserializer)
  .registerTypeAdapter(Time.class, timeDeserializer)
  .create();
```

Thanks to the teacher (I can't tell you one's info). One provided the above code snippets.

For more detail, see [API about `JsonDeserializer`](https://www.javadoc.io/doc/com.google.code.gson/gson/latest/com.google.gson/com/google/gson/JsonDeserializer.html)
