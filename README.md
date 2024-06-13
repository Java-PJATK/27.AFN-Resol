The types of an argument and its corresponding parameter do not have to match exactly. The compiler considers all overloads and looks for the best match. The first criterion is simple: the number of arguments must be equal to the number of parameters (we disregard the so-called var-args here). Then types are analyzed. If there is no perfect match, the compiler tries to find one by widening the type of arguments: for example, the value of an argument of type byte may be widened to short, short to int etc. This is illustrated by the following program, which defines three overloads of function f: with the parameter of type short, int and double.

# Listing 27 AFN-Resol/Resol.java 51

```java
public class Resol {
    static void f(short a) { System.out.println("s=" + a); }
    static void f(int a) { System.out.println("i=" + a); }
    static void f(double a) { System.out.println("d=" + a); }

    public static void main (String[] args) {
        byte ab = 65;
        char ac = 'A';
        short as = 65;
        int ai = 65;
        long al = 65L;
        double ad = 65D;

        System.out.print("byte -> "); f(ab);
        System.out.print("char -> "); f(ac);
        System.out.print("short -> "); f(as);
        System.out.print("int -> "); f(ai);
        System.out.print("long -> "); f(al);
        System.out.print("double -> "); f(ad);
    }
}
```

The program prints:

```
byte -> s=65
char -> i=65
short -> s=65
int -> i=65
long -> d=65.0
double -> d=65.0
```

As one can see, byte has been widened to short (as there is no overload for bytes). There is no overload for chars either, so char has been widened to int. But not to short! Both types have the same size, but char is unsigned, so there are values of this type that are not representable as shorts. Note also that, as there is no overload for longs, it was widened to double, although it changes the type from integral to floating point. Moreover, it is not true that any number of type long can be exactly represented as a double — it’s impossible for longs with more than 16 digits. Precise rules of method call resolution are a bit more complicated, as they have to take into account also inheritance, the so called boxing/unboxing, var-args, etc. — we will cover these topics later.
