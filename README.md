# ๐ฎ Realm with SwiftUI

## ์ ํ๋ธ Stewart Lynch ์ ์์์ ๋ณด๊ณ  ์ ์ฉํด๋ณด๋ ๋ ํฌ์๋๋ค.

- [Part 1](https://youtu.be/nx3KDnqMU0M) - PropertyWrappers 1
- [Part 2](https://youtu.be/54d_0Mp4icA) - PropertyWrappers 2
- [Part 3](https://youtu.be/I6Yl9p_9WwE) - One Many Relationship 1

<br>

## ํ์ต ๋ด์ฉ

### โ๏ธ Realm ์ฌ์ฉํ ๋ชจ๋ธ ํ์ ๋ง๋ค๊ธฐ

- [Object](https://www.mongodb.com/docs/realm-sdks/swift/latest/Extensions/Object.html) ๋ ํ๋กํ ์ฝ์ด ์๋๋ผ `ํด๋์ค`์๋๋ค. Realm ์ ์ฌ์ฉํ `๋ชจ๋ธ ํ์`์ ๊ตฌํํ  ๋, Object ํด๋์ค๋ฅผ ์์์ํต๋๋ค.
> In Realm you define your model classes by subclassing Object and adding properties to be managed.<br>
> You then instantiate and use your custom subclasses instead of using the Object class directly.

- [ObjectKeyIdentifier](https://www.mongodb.com/docs/realm-sdks/swift/latest/Protocols/ObjectKeyIdentifiable.html) ํ๋กํ ์ฝ์ ์ฑํํ๋ฉด ์๋์ผ๋ก [Identifiable](https://developer.apple.com/documentation/swift/identifiable) ํ๋กํ ์ฝ์ ์ค์ํฉ๋๋ค.
  - ๊ทธ๋ฆฌ๊ณ  `@Persisted(primaryKey: true) var id: ObjectId` ์ ๊ฐ์ด, `primaryKey` ์ญํ ์ ํ๋ id ๋ฅผ ์ถ๊ฐํฉ๋๋ค.

- [ObjectId](https://www.mongodb.com/docs/realm-sdks/swift/latest/Classes/ObjectId.html) ํด๋์ค ํ์์ ํ๋กํผํฐ๋ 12-byte ํฌ๊ธฐ์ `unique object identifier`๋ฅผ ์๋์ผ๋ก ์์ฑํฉ๋๋ค. (์๋ ์บก์ฒ์ ๊ฐ์ด)
  - `ObjectId`๋ฅผ ๊ธฐ์ค์ผ๋ก ๋ฐ์ดํฐ๋ฅผ ์ ๋ ฌํ๋ฉด `์์ฑ๋ ์์`์ ๋์ผํ ๊ฒฐ๊ณผ๋ฅผ ์ป์ ์ ์์ต๋๋ค.
  - `ForEach(toDos.sorted(byKeyPath: "id", ascending: false))` -> ์ด๋ ๊ฒ ํ๋ฉด, ์ต๊ทผ ์์ฑ๋ ๊ฒ๋ถํฐ ๋ด๋ฆผ์ฐจ์ ์ ๋ ฌ๋๋ ๊ฑธ ํ์ธํ์ต๋๋ค.
> ObjectIds are intended to be fast to generate.<br>
> Sorting by an ObjectId field will typically result in the objects being sorted in creation order.

<img width="224" alt="image" src="https://user-images.githubusercontent.com/71127966/161750349-10824bfa-cb53-4802-a012-a87badf15a47.png">

- [@Persisted](https://www.mongodb.com/docs/realm-sdks/swift/latest/Structs/Persisted.html) ํ๋กํผํฐ wrapper ๋ Realm ์ ๊ด๋ฆฌ๋ฅผ ๋ฐ๊ณ  ์ถ์ ํ๋กํผํฐ ์์ ๋ถ์ฌ์ค๋๋ค.
> @Persisted is used to declare properties on Object subclasses which should be managed by Realm.

- ์ด๊ฑฐํ(enum) ํ์์ ํ๋กํผํฐ์ `@Persisted`๋ฅผ ๋ถ์ฌ์, Realm ์ ๊ด๋ฆฌ๋ฅผ ๋ฐ๊ณ  ์ถ๋ค๋ฉด, 2๊ฐ์ง ์กฐ๊ฑด์ด ํ์ํฉ๋๋ค.
  - ๋จผ์ , ์ด๊ฑฐํ์ด `raw value(์์๊ฐ)`๋ฅผ ๊ฐ์ ธ์ผ ํฉ๋๋ค. ๊ทธ๋์ผ Realm ์ด ์ ์ฅํ  ์ ์์ต๋๋ค.
  - ๊ทธ๋ฆฌ๊ณ  ๊ทธ ์ด๊ฑฐํ์ด [PersistableEnum](https://www.mongodb.com/docs/realm-sdks/swift/latest/Protocols.html#/s:10RealmSwift15PersistableEnumP) ํ๋กํ ์ฝ์ ์ฑํํด์ผ ํฉ๋๋ค.
> Persisting an enum in Realm requires that it have a raw value and that the raw value by a type which Realm can store.<br>
> The enum also has to be explicitly marked as conforming to this protocol as Swift does not let us do so implicitly.

```swift
import RealmSwift

final class ToDo: Object, ObjectKeyIdentifiable {
    
    @Persisted(primaryKey: true) var id: ObjectId
    @Persisted var name: String
    @Persisted var completed: Bool = false
    @Persisted var urgency: Urgency = .neutral
    
    enum Urgency: Int, PersistableEnum {
        
        case trivial
        case neutral
        case urgent
```
