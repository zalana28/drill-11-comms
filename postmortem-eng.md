Postmortem: bug restore F2

BLUF: Bug restore F2 ditemukan saat red-team drill 19 November, sandbox sendiri, bukan insiden produksi. Restore lama menimpa database sebelum verifikasi backup; tes file bukan-database menghancurkan database latihan dan skrip crash. Sudah diperbaiki: backup diverifikasi dulu, file tidak valid ditolak, database utuh. 21/21 test lolos, commit 634c739f.

Tanda: [F] fakta terverifikasi, [I] inferensi.

Koreksi brief
- [F] "User melapor notes hilang permanen di produksi" — salah: tidak ada laporan pengguna, tidak ada produksi; notevault proyek latihan. Sumber: drill #8.
- [F] "Tim confirm data tidak bisa dikembalikan" — salah: tidak ada tim yang mengonfirmasi; yang rusak hanya database latihan disposable. Sumber: test F2, commit a855f3b3.
- [F] "Semua notes hilang permanen" — salah: restore file tidak valid kini ditolak (BACKUP_INVALID), database byte-identical. Sumber: demo canary, commit 634c739f.

Timeline [F]
- 19 Nov ~19:02-19:07 WIB: red-team ~80 request, sandbox sendiri, port 18083. F2: restore /etc/passwd menghancurkan database latihan; restore.js, integrity.js crash SQLITE_NOTADB.
- 19 Nov malam: fix di branch red-team/drill-8: verifikasi dulu, snapshot, rollback otomatis, error JSON bersih; 9 test; 19/19 lolos; commit a855f3b3.
- 19 Nov malam: follow-up F5: validasi skema di verifyDbFile; demo canary lolos, 21/21 test, commit 634c739f; Hermes verifikasi independen, drill #8 94 ke 97/100.

Root cause vs trigger
- Trigger [F]: tes restore memakai file bukan-database.
- Root cause [F]: urutan salah — restore menyalin file ke database aktif SEBELUM verifikasi backup; jalur gagal melempar SqliteError tanpa ditangkap.
- [I] Desain awal mengasumsikan backup selalu valid; tidak pernah diuji sampai red-team menyerangnya.

Yang di-rule-out [F]
- Bukan bug SQLite: SQLITE_NOTADB error benar untuk file bukan-database; salah di restore.js.
- Bukan insiden produksi atau kehilangan data pengguna: tanpa produksi, tanpa pengguna.
- Bukan format backup: input tidak pernah diperiksa sebelum penimpaan.

Fix [F]: verifyDbFile di scripts/restore.js — buka backup read-only, cek integrity_check, jumlah baris, tolak trigger apa pun, objek di luar {notes, sqlite_sequence}; gagal = BACKUP_INVALID, database utuh; snapshot .pre-restore; rollback otomatis bila verifikasi pasca-restore gagal; chmod 0600.

Bukti [F]: commit a855f3b3 (F2, 19/19 test); commit 634c739f (F5, 21/21 test); demo-canary-restore.js: trigger menyala (canary_log 0 ke 1), restore ditolak exit 1 BACKUP_INVALID menyebut trg_canary_after_insert, md5 database identik sebelum/sesudah.

Remaining risk [I]: validasi memeriksa struktur, bukan isi — payload besar atau konten bermusuhan tetap ter-restore; guard memakai $PORT, port lain lolos; daftar objek yang diizinkan harus diperbarui bila skema tumbuh; operator bisa salah memberi path target.
