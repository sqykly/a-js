# a-js
A delightfully minimal test framework using well-supported features of ES6

At present, it has no dependencies and four useful functions.  The user must provide two functions, pass and fail,
which will receive passing results and failing results, respectively.  Both will receive the result as a string description.
Descriptions for test bodies given to should() and should.have() and for test case generators to a() and that() are optional.
Failure consists of returning false or throwing an exception from the body of should() or should.have(), as well as lack of
the given property of the test case object to should.have().

    a("thing", () => makeThing(), (it) => { testThing(it); }) // "a thing ..."
    a(() => thing(), (it) => { testThing(it); }) // "a thing() ..."
    
    a(() => thing(), (it) => {
        // "a thing() that has done stuff ..."
        that("has done stuff", (it) => it.doStuff(), (it) => { testThing(it); });
        // "a thing() that it.doStuff() ..."
        that((it) => it.doStuff(), (it) => { testThing(it); });
        // "a thing() should be itself"
        should("be itself", (it) => it === it);
        // "a thing() should it === it"
        should(it => it === it);
        
        that("has a property foo = 'bar'", (it) => { it.foo = "bar"; return it; }, (it) => {
          // "a thing() that has a property foo = 'bar' should have foo"
          should.have("foo");
          // "a thing() that has a property foo = 'bar' should have foo == 'baz' (but got 'bar')"
          should.have("foo", "baz");
        });
    });
