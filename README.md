#Java Best Practice

1. Prefer returning Empty Collections instead of Null
	```
	public class getLocationName {
		return (null==cityName ? "": cityName);
	}
	```

2. Use Strings carefully
    If two Strings are concatenated using “+” operator in a “for” loop, then it creates a new String Object, every time. This causes wastage of memory and increases performance time. Also, while instantiating a String Object, constructors should be avoided and instantiation should happen directly
	```
	//Slower Instantiation
	String bad = new String("Yet another string object");

	//Faster Instantiation
	String good = "Yet another string object"
	```

3. Avoid unnecessary Objects
    One of the most expensive operations (in terms of Memory Utilization) in Java is Object Creation. Thus it is recommended that Objects should only be created or initialized if necessary.

    ```
    import java.util.ArrayList;
    import java.util.List;

    public class Employees {

    	private List Employees;

    	public List getEmployees() {

    		//initialize only when required
    		if(null == Employees) {
    			Employees = new ArrayList();
    		}
    		return Employees;
    	}
    }
    ```

4. Dilemma between Array and ArrayList
    Arrays have fixed size but ArrayLists have variable sizes. Since the size of Array is fixed, the memory gets allocated at the time of declaration of Array type variable. Hence, Arrays are very fast. On the other hand, if we are not aware of the size of the data, then ArrayList is More data will lead to ArrayOutOfBoundException and less data will cause wastage of storage space.
    It is much easier to Add or Remove elements from ArrayList than Array
    Array can be multi-dimensional but ArrayList can be only one dimension

5. Check Oddity
    + Bad:
    ```
    public boolean oddOrNot(int num) {
    	return num % 2 == 1;
    }
    ```

    + Good:
    ```
    public boolean oddOrNot(int num) {
    	return (num & 1) != 0;
    }
    ```

6. Difference between single quotes and double quotes

    ```
    public class Haha {
    	public static void main(String args[]) {
    	System.out.print("H" + "a");
    	System.out.print('H' + 'a');
    	}
    }
    ```
    From the code, it would seem return “HaHa” is returned, but it actually returns Ha169. The reason is that if double quotes are used, the characters are treated as a string but in case of single quotes, the char -valued operands ( ‘H’ and ‘a’ ) to int values through a process known as widening primitive conversion. After integer conversion, the numbers are added and return 169.

7. Don't log and throw
    + Bad:
    ```
    catch (Exception ex) {
        logger.warn("I got an exception!", ex);
        throw ex;
    }
    ```

    + Better:
    ```
    catch (Exception ex) {
        logger.warn("I got an exception!", ex);
    }
    ```

8. Clean up with finally
    + Bad:
    ```
    //   - Connection is not closed if sendMessage throws.
    if (receivedBadMessage) {
        conn.sendMessage("Bad request.");
        conn.close();
    }
    ```

    + Good
    ```
    if (receivedBadMessage) {
        try {
            conn.sendMessage("Bad request.");
        } finally {
            conn.close();
        }
    }
    ```

9. String Concatenations
    + Bad:
    ```
    if (result != null && (!result.isEmpty())) {
        String message = I18nService.translate("some message.\n");
        message += I18nService.translate("another message.\n");
        for (int c = 0; c < result.size(); c++) {
            SomeObj obj = result.get(c);
            message += obj.getPK().getId();
            message += " - ";
            message += obj.getPK().getVersion();
            message += " - ";
            message += obj.getDescription();
            message += "\n";
        }
        logger.error(message);
    }
    ```

    + Better:
    ```
    if (result != null && !result.isEmpty()) {
        StringBuilder message = new StringBuilder();
        message.append(I18nService.translate("some message.\n"));
        message.append(I18nService.translate("another message.\n"));

        for (SomeObj obj : result) {
            SomeObjPK pk = obj.getPK();
            message.append(pk.getId()).append(" - ");
            message.append(pk.getVersion()).append(" - ");
            message.append(obj.getDescription()).append("\n");
        }
        logger.error(message.toString());
    }
    ```

10. Loops (Style code)
    + Bad:
    ```
    for(SomeClass someVar:someVars){
      someVar.doSomething();
    }

    for(SomeClass someVar:someVars)
    {
      someVar.doSomething();
    }
    ```

    + Better:
    ```
    for (SomeClass someVar : someVars) {
      someVar.doSomething();
    }

    for (SomeClass someVar : someVars)
      someVar.doSomething();
    ```

11. Use TypedQuery instead of Query
    TypedQuery is just faster, and you don't need to cast.

    + Bad:
    ```
    Query q = em.createQuery("SELECT t FROM Type t where xyz = :xyz");
    q.setParemeter("xyz", xyz);
    List<Type> types = (List<Type>) q.getResultList();
    ```

    + Better:
    ```
    TypedQuery<Type> q = em.createQuery("SELECT t FROM Type t where xyz = :xyz", Type.class)
        .setParemeter("xyz", xyz);
    List<Type> types = q.getResultList();
    ```

12. Use fluent interface in queries (Chain Method)

    + Good:
    ```
    TypedQuery<Type> q = em.createQuery(
        "SELECT t FROM Type t where xyz = :xyz and abc = :abc", Type.class);
    q.setParemeter("xyz", xyz);
    q.setParemeter("abc", abc);
    List<Type> types = q.getResultList();
    ```

    + Better:
    ```
    List<Type> types = em.createQuery(
        "SELECT t FROM Type t where xyz = :xyz and abc = :abc", Type.class)
        .setParemeter("xyz", xyz)
        .setParemeter("abc", abc)
        .getResultList();
    ```

13. Use FetchType.LAZY on relations

    Sometimes you need to relate two entities, and when you use annotations like @ManyToOne and @OneToOne the default property is FetchType.EAGER.

    EAGER collections are fetched fully at the time their parent is fetched. Even if you don't need them
    LAZY on the other hand, means that the collection is fetched only when you try to access them (It's LAZY hum?)
    There's a HUGE slow down performance when you use EAGER. Only use if you have a good reason to do.

    ```
    @ManyToOne(fetch = FetchType.LAZY)
    private Client clients;
    ```