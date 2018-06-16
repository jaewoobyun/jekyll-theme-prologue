---
title: Enumerations
layout: post
---

# Enumerations
- 연관된 값의 그룹에 대해 공통 타입을 정의한 뒤 type-safe 하게 해당 값들을 사용 가능

``` Swift

enum SomeEnumeration {
  // enumeration definition goes here
}

// Enumeration Type 이름은 Pascal Case
// 각 case 는 Camel Case

enum CompassPoint {
  case north
  case south
  case east
  case west
}

enum Planet {
  case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}

var directionToHead1 = CompassPoint.west
directionToHead1 = .east

var directionToHead2: CompassPoint = .north

```

##Matching Enumeration Values

``` Swift
let directionToHead = CompassPoint.south

switch directionToHead {
case .north:
  print("Lots of planets have a north")
case .south:
  print("Watch out for penguins")
case .east:
  print("Where the sun rises")
case .west:
  print("Where the skies are blue")
}


let somePlanet = Planet.earth

switch somePlanet {
case .earth:
  print("Mostly harmless")
default:
  print("Not a safe place for humans")
}
```

## Associated Values

``` Swift

enum Barcode {
  case upc(Int, Int, Int, Int)
  case qrCode(String)
}

var productBarcode = Barcode.upc(8, 85909, 51226, 3)
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
type(of: productBarcode)

print(productBarcode)



switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
  print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
  print("QR code: \(productCode).")
}


switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
  print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
  print("QR code: \(productCode).")
}

```

## Raw Value
- Strings, Characters, or any of the Integer or Floating-point number types
- raw value 는 해당 Enumeration 내에서 반드시 고유한 값이어야 함.

``` Swift
enum ASCIIControlCharacter1: Character {
  case tab = "\t"
  case lineFeed = "\n"
  case carriageReturn = "\r"
}

ASCIIControlCharacter1.tab
ASCIIControlCharacter1.lineFeed
ASCIIControlCharacter1.carriageReturn

ASCIIControlCharacter1.tab.rawValue


print("111\n행바꿈\t탭") // 예제

enum Weekday: Int {
  case sunday, monday, tuesday, wednesday, thursday, friday, saturday
}

Weekday.wednesday
Weekday.wednesday.rawValue
type(of: Weekday.wednesday)

enum WeekdayName: String {
  case sunday, monday, tuesday, wednesday, thursday, friday, saturday
}

WeekdayName.wednesday
WeekdayName.monday.rawValue

```

## Implicitly Assigned Raw Values

``` Swift
enum WeekdayAgain: Int {
  case sunday, monday = 100, tuesday = 101, wednesday, thursday, friday, saturday
}
WeekdayAgain.sunday.rawValue
WeekdayAgain.tuesday.rawValue
WeekdayAgain.wednesday
WeekdayAgain.wednesday.rawValue





enum WeekdayNameAgain: String {
  case sunday, monday = "mon", tuesday = "tue", wednesday, thursday, friday, saturday
}
WeekdayNameAgain.sunday.rawValue
WeekdayNameAgain.monday.rawValue
WeekdayNameAgain.wednesday
WeekdayNameAgain.wednesday.rawValue


//왜 rawValue 를 쓸일이 생길까?
```


## Initializing from a Raw Value

```Swift

enum PlanetIntRaw: Int {
  case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}

let possiblePlanet = PlanetIntRaw(rawValue: 7)
let positionToFind = 11 //7 => "not a safe ~", 3 => "Mostly harmless"

if let somePlanet = PlanetIntRaw(rawValue: positionToFind) {
  switch somePlanet {
  case .earth:
    print("Mostly harmless")
  default:
    print("Not a safe place for humans")
  }
} else {
  print("There isn't a planet at position \(positionToFind)")
}
