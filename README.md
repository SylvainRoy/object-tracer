objectTracer
============

Small library to log the activity on a given object.


What is this?
=============

Did you ever want to trace the activity on a given object in the process of an obscure function call?
Well, I did.

Python allows quite a few neat tricks on objects and classes. So, it is easy to instrument an object to trace all it's activity.

That being said, it took me some time to get it 'right'.
So, I took the extra step to make it a standalone module.

It allows one to track the activity (i.e. method calls, attribute get/set/del) of one or several objects for a limited period of time.

Exactly what you need if you want to trace what an obscure function of an even more obscure module does to this object that you pass in parameter.



How does it work?
=================

Objecttracer is not intrusive. You only have to instrument the object you want to trace.

Let say you want to trace all the activity of the object 'myobject' in the call to 'mysecretfunction', this can be as simple as:

	import objecttracer

	objecttracer.instrument(a, 'a')
	# All activity on 'a' will be printed on stdout from this point

	mysecretfunction(a)

	objecttracer.clean(a)
	# No more tracking of 'a'


The other objects of the class are not impacted by the instrumentation.
The behaviour of 'a' does not change at all when instrumented.

Once you have cleaned the object, it's like nothing ever happened.

You should look at the unit tests to see more example of usage.

What do I get?
==============

Let say that:

 * 'mysecretfunction' calls the method 'mymethod', with a single parameter equal to 5, of the object 'a'.

 * 'mymethod' gets the value of the attribute 'myattribute' and adds 5 to it.

Here is what you will get on stdout (or any other stream given to 'instrument'):

	> a.mymethod(5)         # Entering method 'mymethod' of a with parameter 5
	  a.myattribute > 12    # a.myattribute accessed, value returned is 12
	  a.myattribute < 17    # a.myattribute set to 17
	< None                  # Leaving function 'mymethod' with return value equal to None

Here is an example of something slightly more complex:

	> a.add(1)
	  a.a > 5
	  a.a < 6
	  a.a > 6
	< 6
	> a.addn(2, 2)
	  > a.add(2)
	    a.a > 6
	    a.a < 8
	    a.a > 8
	  < 8
	  > a.add(2)
	    a.a > 8
	    a.a < 10
	    a.a > 10
	  < 10
	< None


Any limitation?
===============

Tested with python 3.7 and above.
Should work with the all python 3 releases.


Unit tests?
===========

Yeap. Just run the following command:

	python objecttracer_test.py
