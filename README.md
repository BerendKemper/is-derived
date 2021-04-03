# is-derived
Walks up the Target's prototype chain to find if it was derived from Source. Might be usefull to extend sub-classes from a class interface.

```javascript
// Case 1:
const ApiRegister = function moduleWrapper() { // namespcace scope or such
	class ApiRecord { };
	return class ApiRegister { // this is a simple class interface
		static #ApiRecord = ApiRecord; // this is a sub-class used in the class interface
		static get ApiRecord() { // this is used extend a new class from the sub-class
			return ApiRegister.#ApiRecord;
		};
		static set ApiRecord(OwnApiRecord) { // this is used to overwrtie the existing sub-class
			if (!isDerived(OwnApiRecord, ApiRecord)) // it must be derived from the initial sub-class
				throw TypeError(`The parameter OwnApiRecord is not derived from ApiRecord`);
			ApiRegister.#ApiRecord = OwnApiRecord;
		};
	};
}();
ApiRegister.ApiRecord = class OwnApiRecord {}; // throws an error
ApiRegister.ApiRecord = class OwnApiRecord extends ApiRegister.ApiRecord {}; // works
// 
//
// Case 2:
const LocaleTimezoneDate = require("locale-timezone-date"); // is extended from Date
class TaskClock {
	static #DateModel = Date; 
	static get DateModel() { // this is used extend a new class from the sub-class
		return TaskClock.#DateModel;
	};
	static set DateModel(OwnDateModel) { // this is used to overwrtie the existing sub-class
		if (!isDerived(OwnDateModel, Date)) // it must be derived from the initial sub-class
			throw TypeError(`The parameter OwnApiRecord is not derived from ApiRecord`);
		TaskClock.#DateModel = OwnDateModel;
	};
};
TaskClock.DateModel = class MyDate {}; // throws an error
TaskClock.DateModel = LocaleTimezoneDate; // works
```