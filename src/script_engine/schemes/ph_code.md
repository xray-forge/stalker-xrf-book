# ph_code

`ph_code` opens a numeric input window for a physical object and evaluates condlists for entered codes. It is mainly
used for code locks.

## Parameters

| Field                                   | Type                 | Required                    | Default       | Description                                         |
| --------------------------------------- | -------------------- | --------------------------- | ------------- | --------------------------------------------------- |
| `tips`                                  | string               | no                          | `st_codelock` | Tip text assigned to the object.                    |
| `code`                                  | number               | no                          | `null`        | Single accepted numeric code.                       |
| `on_code`                               | condlist             | required when `code` is set | `null`        | Condlist evaluated when entered text equals `code`. |
| `on_check_code1`, `on_check_code2`, ... | `string \| condlist` | no                          | empty         | Per-code condlists used when `code` is not set.     |

The section also supports common switch fields, but code input itself is handled through the use callback.

## Usage

There are two modes:

| Mode           | Config                           | Behavior                                                    |
| -------------- | -------------------------------- | ----------------------------------------------------------- |
| Single code    | `code` plus `on_code`            | Entering the matching number evaluates `on_code`.           |
| Multiple codes | numbered `on_check_codeN` fields | Entering a matching text key evaluates that key's condlist. |

Current implementation evaluates the selected condlist for effects and info portion changes. It does not explicitly
switch to the returned section from the code manager.

## Example

```ini
[logic]
active = ph_code@lock

[ph_code@lock]
tips = st_enter_code
code = 1234
on_code = nil %+door_code_entered =play_sound(code_ok)%
```

Multiple code form:

```ini
[ph_code@multi]
on_check_code1 = 1111 | nil %+first_code_entered%
on_check_code2 = 2222 | nil %+second_code_entered%
```

## Notes

- Activation makes the object script-usable by setting `nonscript_usable` to `false`.
- Deactivation clears the tip text.
- Use `ph_door` or another section's switch logic when a successful code should open or change an object state.
