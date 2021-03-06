# merge-pojos

Core library to merge/patch Java Pojos.


merge-pojos is a simple and easy-to-use library which provides a neat and customizable patch/merge mechnisim for java objects (i.e., Pojos). Such fucntionality is typically needed while developing webservices to support an HTTP Patch method.

For example, lets assume we have two instances of `DummyPojo.class` and we would like to patch `oldPojo` with some data from `newPojo`, here is a code snippet which shows how merge-pojos can be used to do that:

```
DummyPojo newPojo = new DummyPojo(123);
DummyPojo oldPojo = new DummyPojo(456);
MergeFactory factory = new MergeFactoryImpl();
DummyPojo resultPojo = factory.mergeFacade().merge(newPojo, oldPojo);
```

We can also define different merge rules and strategies at different granularities. For example, if we would like to merge only non-null fields of our `newPojo`, we can specify a SOURCE_NOT_NULL strategy as follows:
```
DummyPojo resultPojo = factory.mergeFacade().merge(newPojo, oldPojo, SOURCE_NOT_NULL);
```
Sometimes, we would also like to ignore any empty maps, arrays or collections. This is achievable using SOURCE_NOT_EMPTY strategy. We note that SOURCE_NOT_NULL is actually the default merge strategy which will be applied when no strategy is provided. On the contrary, we may wish to patch only fields which are null in the `oldPojo`. That would be achieved that with TARGET_IS_NULL strategy as shown below:
```
DummyPojo resultPojo = factory.mergeFacade().merge(newPojo, oldPojo, TARGET_IS_NULL);
```

The examples above on apply merge strategies globally on all fields. However, finer merge rules can be established on fields using the _`@MergeStrategy`_ annotation on respective fields. If a field is annotated with _`@MergeStrategy`_, then such annotation takes precedence over any global strategies. Below is a list of some of the intial merge strategies provided by merge-pojos:

* SOURCE: Returns the source always.
* SOURCE_NOT_EMPTY: A field in source overwrites its corresponding in target if the field in source is not empty.
* SOURCE_NOT_NULL: A field in source overwrites its corresponding in target if the field in source is not null.
* SOURCE_NOT_NULL_NOT_EMPTY: A field in source overwrites its corresponding in target if the field in source is not null and not empty.
* TARGET: Returns the target always.
* TARGET_EMPTY: A field in source overwrites its corresponding in target if the field in target is empty.
* TARGET_IS_NULL:  A field in source overwrites its corresponding in target if the field in target is null.
* TARGET_IS_NULL_OR_EMPTY: A field in source overwrites its corresponding in target if the field in target is  null or empty.


merge-pojos also allows customizing merge of certain fields. For instance, say `DummyPojo.class` has a field __firstName__ of type String. Then we can customize the merge of such field as follows:
```
DummyPojo resultPojo = factory.mergeFacade().customize(String.class, "firstName",  new CustomMerge<String>() {

			public String merge(String source, String target) {
				return "my dummy pojo's name";
			}
		}).merge(pojo1, pojo2);
```
Now, the value of the field __firstName__ in our `resultPojo` will be _"my dummy pojo's name"_.


}

