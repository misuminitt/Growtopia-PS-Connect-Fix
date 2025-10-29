# Growtopia PS Connect — Reset-to-Default Method

Panduan ini menjelaskan cara **mengembalikan semua pengaturan jaringan & client ke kondisi DEFAULT**, lalu **mengisi file HOSTS dari owner** agar Growtopia EXE client bisa terhubung ke private server.

> Metode ini terbukti berhasil: setelah reset ke default, cukup isi HOSTS dari owner, lalu game dapat login ke private server.

---

## Spec Laptop yang digunakan
- Windows 10/11
- Growtopia EXE client (non-Steam)
- Akses Administrator (untuk edit HOSTS & pengaturan jaringan)
- Baris HOSTS dari owner server

---

## Langkah-langkah

1) **Edit HOSTS dengan Notepad (Admin)**
   - Buka:
     ```
     notepad C:\Windows\System32\drivers\etc\hosts
     ```
   - Tempel baris yang diberikan owner (contoh Nether PS):
     ```
     15.235.166.218 growtopia1.com
     15.235.166.218 growtopia2.com
     15.235.166.218 www.growtopia1.com
     15.235.166.218 www.growtopia2.com
     15.235.166.218 RvLnd.here
     ```
   - **Save**.

2) **Hapus override Growtopia (jika pernah pakai)**
   Jalankan PowerShell **Run as Administrator**:
   ```powershell
   Remove-Item "$env:LOCALAPPDATA\Growtopia\growtopia_override.cfg" -Force -ErrorAction SilentlyContinue
   dir "$env:LOCALAPPDATA\Growtopia"


Jika masih ada file override lain → **hapus juga**.

3. **Pastikan DNS Client Service aktif**

   ```powershell
   net start dnscache
   ```

   Jika muncul `already running` → aman.

4. **Kembalikan DNS ke mode otomatis (IPv4)**

   * `Win + R` → `control netconnections`
   * Klik kanan **Wi-Fi/Ethernet** → **Properties**
   * **Internet Protocol Version 4 (IPv4)** → **Properties**
   * Pilih **Obtain DNS server address automatically** → **OK**

5. **Aktifkan kembali IPv6 (bila sebelumnya dinonaktifkan)**

   ```powershell
   reg delete "HKLM\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters" /v DisabledComponents /f
   ```

6. **Hapus firewall rules custom (jika pernah ditambah)**

   ```powershell
   netsh advfirewall firewall delete rule name="GT TCP 6969 OUT"
   netsh advfirewall firewall delete rule name="GT TCP 6969 IN"
   netsh advfirewall firewall delete rule name="Growtopia TCP 443 OUT"
   netsh advfirewall firewall delete rule name="Growtopia TCP 443 IN"
   netsh advfirewall firewall delete rule name="Growtopia UDP"
   netsh advfirewall firewall delete rule name="Growtopia UDP2"
   ```

   (Jika “rule not found”, itu normal.)

7. **Flush DNS**

   ```powershell
   ipconfig /flushdns
   ```

8. **Restart Windows (Wajib)**
   Setelah restart, buka Growtopia dan login.

---

## Verifikasi

Sebelum membuka game, jalankan:

```powershell
nslookup growtopia1.com
```

Jika mengarah ke IP server private (contoh `15.235.166.218`), artinya HOSTS sudah bekerja.
