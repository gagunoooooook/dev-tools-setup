# Changelog

## 1.4.2

- Default benchmark window raised to 25 min (RandomBench needs ~60s warm-up
  before measurements stabilise; previous 5-min default included too much
  noise from the warm-up window).
- ARM64 build fix (cmake toolchain detection).
- Reset nonce stability fix.

## 1.4.1

- Initial open-sourced release.
