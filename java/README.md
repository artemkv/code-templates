##  "Hello World"

```
package net.artemkv;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

## StringBuilder

```
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(", ");
sb.append("World!");
String s = sb.toString();
```

## String.format

```
String city = "Barcelona";
int year = 2020;
double pi = 3.1415;

String cityFormatted = String.format("City: %s", city); // City: Barcelona
String yearFormatted = String.format("Year: %d", year); // Year: 2020
String piFormatted = String.format("Pi: %05.2f", pi); // Pi: 03.14
```

## "For" loop

```
int[] arr = {1, 2, 3, 4, 5};
for (int i = 0; i < arr.length; i++) {
	System.out.println(arr[i]);
}
```

## Array to list to array

```
// list to array

List<String> list = new LinkedList<>();
list.add("Hello");
list.add(", ");
list.add("World!");
String[] arr = list.toArray(new String[0]);


// array to list

String[] arr = { "Hello", ", ", "World!" };
List<String> list = Arrays.asList(arr); // fixed-size list
```

## Class

```
package net.artemkv;

public class Person {
    private final String firstName;
    private final String lastName;
    private final int age;

    public Person(String firstName, String lastName, int age) {
        if (firstName == null) {
            throw new IllegalArgumentException("firstName");
        }
        if (lastName == null) {
            throw new IllegalArgumentException("lastName");
        }

        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public int getAge() {
        return age;
    }

    public boolean isAdult() {
        return age > 21;
    }
}
```

## equals, hashCode

```
@Override
public boolean equals(Object o) {
	if (o == this) {
		return true;
	}
	if (!(o instanceof P)) {
		return false;
	}
	P other = (P) o;
	return id.equals(other.id);
}

@Override
public int hashCode() {
    int result = 17;
	result = 31 * result + x;
	result = 31 * result + y;
	return result;
}
```

## Inheritance

```
package net.artemkv;

public abstract class Animal {
    private final String name;

    public Animal(String name) {
        this.name = name;
    }

    public final String getName() {
        return name;
    }

    public abstract String makeSound();
}

public class Cat extends Animal {
    public Cat() {
        super("cat");
    }

    @Override
    public String makeSound() {
        return "meeew";
    }
}

Animal cat = new Cat();
System.out.printf("%s said %s", cat.getName(), cat.makeSound());
```

## Inner classes

```
package net.artemkv;

import java.util.LinkedList;
import java.util.List;

public class TaskScheduler {
    private static class TaskDefinition {
        public final String name;
        public final Runnable task;

        public TaskDefinition(String name, Runnable task) {
            this.name = name;
            this.task = task;
        }
    }

    private List<TaskDefinition> tasks = new LinkedList<>();

    public TaskScheduler() {
    }

    public void addTask(String name, Runnable task) {
        tasks.add(new TaskDefinition(name, task));
    }
}
```

## Builder pattern

```
package net.artemkv;

public class PersonBuilder {
    private String firstName;
    private String lastName;

    public PersonBuilder() {
    }

    public PersonBuilder withFirstName(String firstName) {
        if (firstName == null) {
            throw new IllegalArgumentException("firstName");
        }

        this.firstName = firstName;
        return this;
    }

    public PersonBuilder withLastName(String lastName) {
        if (lastName == null) {
            throw new IllegalArgumentException("lastName");
        }

        this.lastName = lastName;
        return this;
    }

    public String getFirstName() {
        return this.firstName;
    }

    public String getLastName() {
        return this.lastName;
    }

    public Person build() {
        return new Person(this);
    }
}

public class Person {
    private final String firstName;
    private final String lastName;

    Person(PersonBuilder builder) {
        this.firstName = builder.getFirstName();
        this.lastName = builder.getLastName();
    }
}
```

## Singleton

```
package net.artemkv;

public final class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

## Enums

```
package net.artemkv;

public enum Component {
    RED("Red"),
    GREEN("Green"),
    BLUE("Blue");

    private final String name;

    Component(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return name;
    }
}
```

## Exception handling

```
package net.artemkv;

import java.io.PrintWriter;
import java.io.StringWriter;

public class ExceptionsExample {
    public static void test() {
        try {
            String s = null;
            int x = s.length();
        } catch (Exception ex) {
            handle(ex);
        } finally {
            System.out.println("always executed");
        }
    }

    private static void handle(Exception e) {
        // full stack trace
        StringWriter stringWriter = new StringWriter();
        PrintWriter printWriter = new PrintWriter(stringWriter);
        e.printStackTrace(printWriter);
        String details = stringWriter.toString();
        System.out.println(details);

        // go through the stack trace
        StackTraceElement[] frames = e.getStackTrace();
        for (StackTraceElement frame : frames) {
            System.out.println(frame.toString());
        }
    }
}
```

## Custom exceptions

```
package net.artemkv;

public class DownloadCancelledException extends RuntimeException {
    public DownloadCancelledException() {
    }
    public DownloadCancelledException(String message) {
        super(message);
    }
}
```

## Try with resource - manual

```
InputStreamReader reader = null;
try {
	// reader = new InputStreamReader(...);
	// TODO: use reader
} catch (IOException e) {
	throw new RuntimeException(e);
}
finally {
	try {
		if (reader != null) {
			reader.close();
		}
	}
	catch (IOException e) {
		// Ignore
		Log.i("XXXXX", "Error closing reader");
	}
}
```

## Functional interfaces

```
// non-pure
Supplier<Integer> getRandom = () -> new Random().nextInt();
Consumer<Integer> log = x -> System.out.println(x);

// can be pure
Function<Integer, String> toString = x -> x.toString();
UnaryOperator<Integer> duplicate = x -> x * 2;
Predicate<Integer> isEven = x -> x % 2 == 0;
```

## Futures

```
package net.artemkv;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.Optional;
import java.util.concurrent.CompletableFuture;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.stream.Collectors;

public class Futures {
    public static CompletableFuture schedule() {
        Supplier<Optional<List<Path>>> readDir = () -> {
            Path dir = Paths.get("d://temp");
            try {
                List<Path> paths = Files.list(dir).collect(Collectors.toList());
                return Optional.of(paths);
            } catch (IOException e) {
                return Optional.empty();
            }
        };

        Function<Optional<List<Path>>, Optional<List<Path>>> toFileNames = filePaths -> {
            Function<Path, Path> toFileName = filePath -> filePath.getFileName();
            Function<List<Path>, List<Path>> toListOfFileNames =
                list -> list.stream().map(toFileName).collect(Collectors.toList());
            return filePaths.map(toListOfFileNames);
        };

        Consumer<Optional<List<Path>>> printDir = fileNames -> {
            if (fileNames.isPresent()) {
                fileNames.get().stream().forEach(System.out::println);
            } else {
                System.out.println("Could not retrieve directory list");
            }
        };

        CompletableFuture future = CompletableFuture
            .supplyAsync(readDir)
            .thenApply(toFileNames)
            .thenAccept(printDir);
        return future;
    }
}

CompletableFuture future = Futures.schedule();
future.join();
```

## Synchronized

```
package net.artemkv;

import java.util.LinkedList;
import java.util.List;
import java.util.stream.Stream;

public class ThreadSafeList<T> {
    private final List<T> list = new LinkedList<>();
    private final Object lock = new Object();

    public void add(T item) {
        synchronized (lock) {
            if (!list.contains(item)) {
                list.add(item);
            }
        }
    }
}
```

## Safe lazy initialization

```
package net.artemkv;

public final class SafeLazyInitialization {
    private static SafeLazyInitialization instance;

    private SafeLazyInitialization() {
    }

    public synchronized static SafeLazyInitialization getInstance() {
        if (instance == null) {
            instance = new SafeLazyInitialization();
        }
        return instance;
    }
}
```

## Safe lazy initialization - holder class idiom

```
package net.artemkv;

public class SingletonLazyInitialization {
    private static class ResourceHolder {
        public static final SingletonLazyInitialization resource = costlyIdempotentOperation();

        private static SingletonLazyInitialization costlyIdempotentOperation() {
            return new SingletonLazyInitialization();
        }
    }

    public static SingletonLazyInitialization getInstance() {
        return ResourceHolder.resource;
    }
}
```

## Safe lazy initialization - no synchronization

```
package net.artemkv;

import java.util.concurrent.atomic.AtomicReference;

public final class SingletonLazyInitialization {
    private static AtomicReference<SingletonLazyInitialization> instance = new AtomicReference<>();

    private SingletonLazyInitialization() {
    }

    public static SingletonLazyInitialization getInstance() {
        // no synchronization required if already created
        if (instance.get() != null) {
            return instance.get();
        }

        // costlyIdempotentOperation may be executed twice...
        SingletonLazyInitialization temp = costlyIdempotentOperation();
        instance.compareAndSet(null, temp);

        // ...but every thread gets the same instance
        return instance.get();
    }

    private static SingletonLazyInitialization costlyIdempotentOperation() {
        return new SingletonLazyInitialization();
    }
}
```

## Safe lazy initialization - double-check

```
package net.artemkv;

public class DoubleCheck {
    private static volatile DoubleCheck instance;
    private static final Object lock = new Object();

    private DoubleCheck() {
    }

    public static DoubleCheck getInstance() {
        DoubleCheck result = instance;
        if (result == null) {
            synchronized(lock) {
                result = instance;
                if (result == null) {
                    result = costlyOperation();
                    instance = result;
                }
            }
        }
        return result;
    }

    private static DoubleCheck costlyOperation() {
        return new DoubleCheck();
    }
}
```

## Interrupted exception

```
try {
	sleep(delay);
} catch (InterruptedException e) {
	// restore interrupted status
	Thread.currentThread().interrupt();
}
```

## Paths and files

```
Path path = Paths.get("d://temp");
Files.exists(path);
Files.isDirectory(path);

Path dir = Paths.get("d://temp");
try {
	Files.list(dir).forEach(System.out::println);
} catch (IOException e) {
	e.printStackTrace();
}

Path doc = Paths.get("d://temp/test.txt");
try {
	Files.lines(doc).forEach(System.out::println);
} catch (IOException e) {
	e.printStackTrace();
}
```

## Buffered reader

```
Path path = Paths.get("d://temp//test.txt");
try (BufferedReader r = Files.newBufferedReader(path)) {
	r.lines().forEach(System.out::println);
} catch (IOException e) {
	System.out.println("Something went wrong: " + e.toString());
}
```

## Channels and buffers

```
Path path = Paths.get("d://temp//test.txt");
try (SeekableByteChannel channel = Files.newByteChannel(path, StandardOpenOption.READ)) {
	ByteBuffer buffer = ByteBuffer.allocateDirect(1024);
	while (channel.read(buffer) > 0) {
		buffer.flip();
		while (buffer.hasRemaining()) {
			System.out.print(buffer.get() + ",");
		}
		buffer.clear();
	}
} catch (IOException e) {
	System.out.println("Something went wrong: " + e.toString());
}
```

## Streams

```
// Quick list creation
List<Integer> list = Arrays.asList(1, 2, 3);

// Array to stream
int[] arr = new int[] {1, 2, 3};
Arrays.stream(arr).forEach(x -> System.out.println("Number is " + x));

// Stream literal (return:))
Stream.of(1, 2, 3).forEach(x -> System.out.println("Number is " + x));

// Skip + limit
IntStream.iterate(1, x -> x + 1)
	.skip(4)
	.limit(8)
	.forEach(System.out::println);

// Int streams, double streams
IntStream.iterate(1, x -> x + 1)
	.mapToDouble(x -> x * 2.0)
	.forEach(System.out::println);

// Range
IntStream.range(0, 5)
	.forEach(System.out::println);

// Peek (tap)
IntStream.range(0, 5)
	.peek(x -> System.out.println("Number is " + x))
	.forEach(System.out::println);
	
// Optional
Optional<String> val = Stream.of("aaa", "bbb", "ccc").findFirst();
Optional<String> str = val.map(a -> "First value is " + a);

// Joining collector
String str = Stream.of(1, 2, 3, 4, 5)
	.map(x -> x.toString())
	.collect(Collectors.joining(" "));
```
