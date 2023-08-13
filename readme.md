Example:

<p>Main testing method:</p>

```java
public class Main {
    public static void main(String[] args) throws Exception {
        AnnotationObjectMapper.initialize();

        System.out.println("Mapped annotation properties:");
        System.out.println("intValue: " + TargetClass.intValueProp);
        System.out.println("stringValue: " + TargetClass.stringValueProp);

        ExampleSourceClass sourceObject = new ExampleSourceClass(23.72);

        TargetClass mappedObject = AnnotationObjectMapper.getInstance().mapObjectTo(sourceObject, TargetClass.class);

        System.out.println("Mapped functions:");
        System.out.println("Sum:" + mappedObject.sumMethod(2, 2));
        System.out.println("getHelloWorld:" + mappedObject.getHelloWorldMethod());

        System.out.println("Mapped field:");
        System.out.println(mappedObject.getField());
    }
}
```

<p>Output</p>

```java
Mapped annotation properties:
intValue: 8302
stringValue: default string value
Mapped functions:
Sum:4
getHelloWorld:Hello world
Mapped field:
23.72
```

<p>ExampleSourceClass code</p>

```java
@TestClassAnnotation
@TestClassAnnotation2(intValue = 8302)
public class ExampleSourceClass {
    @TestField
    private final Double doubleValue;

    public ExampleSourceClass(Double doubleValue) {
        this.doubleValue = doubleValue;
    }

    @TestSumMethod
    public int sum(int a, int b) {
        return a + b;
    }

    @TestGetHelloWorld
    public String getHelloWorld() {
        return "Hello world";
    }
}
```

<p>Target class code</p>

```java
@MappingTarget
public abstract class TargetClass {

    @MapAnnotationProperty(annotationPropName = "stringValue", sourceClass = ExampleSourceClass.class, targetAnnotation = TestClassAnnotation.class)
    public static String stringValueProp;

    @MapAnnotationProperty(annotationPropName = "intValue", sourceClass = ExampleSourceClass.class, targetAnnotation = TestClassAnnotation2.class)
    public static int intValueProp;

    @MapMethod(annotatedWith = TestSumMethod.class)
    public abstract int sumMethod(int a, int b);

    @MapMethod(annotatedWith = TestGetHelloWorld.class)
    public abstract String getHelloWorldMethod();

    @MapField(annotatedWith = TestField.class)
    public abstract Double getField();
}

```