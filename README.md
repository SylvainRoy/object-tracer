objectTracer
============

Small library to log the activity on a given object.


What is this?
=============

Did you ever want to trace the activity on a given object in the process of an obscure function call?
Well, I did.

Python allows quite a few neat tricks on object and class. So, it is easy to instrument an object to trace all it's activity.

That being said, it took me some time to get it 'right'.
So, I took the extra step to make it a standalone module.

It allows one to track the activity (i.e. method calls, attribute get/set/del) of one or several object for a limited period of time.

Exactly what you need if you want to trace what an obscure function of an even more obscure module does to this object that you pass in parameter.



How does it work?
=================

ObjectTracer is not intrusive. You only have to instrument the object you want to trace.

Let say you want to trace all the activity of the object 'myobject' in the call to 'mysecretfunction', this can be as simple as:

	import objectTracer

	objectTracer.instrument(a, 'a')
	# All activity on 'a' will be printed on stdout from this point

	mysecretfunction(a)

	objectTracer.clean(a)
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



Any limitation?
===============

This has been tested with python 2.7 only.
Does not work with python 3 but it should be very easy to fix (I just do not have any python 3 install at hand).

Also, this only work for ''new style'' object (you know the ones that inherit from 'object').



Unit tests?
===========

Yeap. Just run the following command:

	python objectTracer-test.py
