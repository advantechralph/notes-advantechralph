#### BootROM Boot Flow
  1. Upon release of reset, BPMP executes from IROM
  2. BOOTROM establishes root keys
  3. Copies Boot Configuration Table (BCT) including RSA2048 Public Key (PK) into SysRAM
  4. Validates SHA256 hash vs RSA2048 public key modulus (PK vs PKH)
  5. Loads validated public key modulus (PK) into hardware crypto Security Engine (SE) keyslot
  6. Loads 128b AES Secure Boot Key (SBK) and generates Secure Storage Key (SSK), which is written into SE keyslot
  7. Load KEK0/1/2 keys in the key slot
  8. BOOTROM configures hardware based on authenticated BCT
  9. Authenticates BCT signature with public key
  10. Configures boot device based on BCT
  11. BOOTROM authenticates and jumps to MB1
  12. Copies Boot Loader (MB1) into SysRAM
  13. Authenticates MB1 signature with public key
  14. Locks down security configuration
  15. Jumps to MB1
  16. Similar loading and authentication flow for MB2 except loaded by MB1 to SDRAM

#### Later Stages Boot Details
  1. MB2, running on BPMP, loads firmware binaries to DRAM and SysRam
  2. MB2 authenticates the firmware that is loaded
  3. MB2 initializes required CPU complex
  4. MB2 copies TrustedOS (TOS) into TZ DRAM
  5. MB2 authenticates TOS in TZ DRAM
  6. MB2 copies CBoot into DRAM
  7. MB2 authenticates CBoot
  8. CPU complex begins TOS and CBoot execution
  9. MB2 hand-off control to BPMP firmware


