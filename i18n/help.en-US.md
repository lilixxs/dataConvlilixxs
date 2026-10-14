## 01 Quick Input & Conversion Trigger

- **Enter in any row:** Type a value in any data type box.
- **Trigger conversion:** Press `Enter` or click the `↵` icon on the right.
- **Sync all types:** Compute and update the other 9 data types automatically.

> // Supports 3 input formats (case-insensitive)
| Format | Examples |
| :--- | :--- |
| Decimal / Scientific | `200` `-32768` `3.14e-2` |
| Hexadecimal (0x prefix) | `0x12345678` `0xFF` |
| Binary (0b prefix) | `0b11001000` `0b0101` |

## 02 Endianness Diagram

- **Byte labels:** Canonical Big-Endian (MSB first) labels bytes as **A, B, C, D...**
- **Per-row selection:** Select endianness independently on each row to reorder its display immediately.
- **Use case:** Debug Modbus registers or cross-device STM32 byte ordering.

> // Example for 32-bit value 0x12345678:
| Endianness | Byte Order |
| :--- | :--- |
| ABCD (Big-Endian) | `A:12` `B:34` `C:56` `D:78` |
| DCBA (Little-Endian) | `D:78` `C:56` `B:34` `A:12` |
| BADC (Byte Swap) | `B:34` `A:12` `D:78` `C:56` |
| CDAB (Word Swap) | `C:56` `D:78` `A:12` `B:34` |

## 03 Visual Byte Spacing & Space-Free Copy

- **Byte spacing:** Hex and Binary modes insert visual margins between bytes for readability.
- **Space-free copy:** Click the `[COPY_ICON]` button (or select and copy) to strip visual gaps and show `[OK_MARK]`, ready to paste into C code.

> COPY_DEMO: Display: 0x12 34 56 78 | ──Copy──► | 0x12345678

## 04 STM32 Conversion & Range Rules

- **Float to integer:** Truncates toward zero per C standard
  SUB: Example: `12.9 → 12`, `-12.9 → -12`
- **Target overflow:** Displays `[WARN:Out of range]` when the source value exceeds the target range
  SUB: Example: When `uint8 = 200`, `int8` shows Out of range
- **Source out of bounds:** Highlights the status cell in red when the input exceeds the row range
  SUB: Switching radix during an error or overflow state never triggers an invalid conversion
