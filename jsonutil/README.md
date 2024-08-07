# gmlewis/jsonutil

This is a fork of: https://github.com/mizchi/moonbit-libs/tree/main/json

JSON utilities

This version has been updated for MoonBit:

```bash
$ moon version --all
moon 0.1.20240806 (bd4b63d 2024-08-06) ~/.moon/bin/moon
moonc v0.1.20240806+533ed16a3 ~/.moon/bin/moonc
moonrun 0.1.20240716 (08bce9c 2024-07-16) ~/.moon/bin/moonrun
```

To install:

```bash
$ moon add gmlewis/jsonutil
```

moon.pkg.json

```json
{
  "import": [
    "gmlewis/jsonutil"
  ]
}
```

## Usage

```rust
fn main {
  // use moonbitlang/core/json
  let j = @json.parse?(
    #|{
    #|  "a": 1.1,
    #|  "b": [1, 2, 3],
    #|  "c": {
    #|    "d": 4
    #|  },
    #|  "d": null,
    #|  "e": true,
    #|  "f": false
    #|}
    ,
  ).unwrap()
  // like JSON.stringify({}, null, 2)
  let s = stringify(j, spaces=2, newline=true)
}
```

Implment `ToJson` for struct

```rust
priv struct TestTree {
  val : Int
  child : TestTree?
}

impl ToJson for TestTree with to_json(self) {
  from_entries([("val", self.val), ("child", self.child)])
}

test "to_json on TestTree" {
  let v : TestTree = { val: 1, child: Some({ val: 2, child: None }) }
  let j = to_json(v)
  inspect!(
    j,
    content=
      #|Object("val": Number(1.0), "child": Object("val": Number(2.0), "child": Null})})
    ,
  )
  inspect!(
    stringify(j),
    content="{\"val\":1,\"child\":{\"val\":2,\"child\":null}}",
  )
}
```

(I'm researching derive trait)

## Related works

- https://mooncakes.io/docs/#/peter-jerry-ye/json/

## LICENSE

MIT
