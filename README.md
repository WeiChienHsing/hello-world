	Field f = ClassLoader.class.getDeclaredField("classes");
		f.setAccessible(true);

		ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
		Vector<Class> classes =  (Vector<Class>) f.get(classLoader);
		   for (Iterator iter = classes.iterator(); iter.hasNext();) {
	            System.out.println("   Loaded " + iter.next());
	        }
