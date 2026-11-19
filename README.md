# Drill #11 — Komunikasi tiga audiens (insiden F2 notevault)

Tiga dokumen tentang bug restore F2 (restore menimpa database sebelum memverifikasi backup), masing-masing untuk audiens berbeda. Teks biasa, tanpa code fence. Jumlah kata dihitung dengan Python `len(text.split())`.

- incident-user.md — untuk zalana (user), 114 kata dari limit 120.
- postmortem-eng.md — untuk engineer, 392 kata dari limit 400. Reasoning-first/BLUF, penanda [F] fakta terverifikasi dan [I] inferensi.
- handoff-agent.md — untuk agent baru yang mengambil alih notevault, 245 kata dari limit 300.

## Koreksi brief (berlaku untuk ketiga dokumen)

Brief drill ini sengaja memuat fakta salah. Yang benar, dari record terverifikasi:

- Bukan insiden produksi: F2 ditemukan saat red-team drill 19 November 2026 di sandbox sendiri (~80 request, port 18083). Tidak ada laporan pengguna, tidak ada produksi, tidak ada pengguna.
- Tidak ada data hilang permanen: file yang rusak hanya database latihan (disposable, isinya catatan tes). Setelah perbaikan, restore file tidak valid ditolak dan database dibiarkan utuh.
- Tidak ada tim yang mengonfirmasi kehilangan data.

## Bukti (record terverifikasi)

- Fix F2: branch red-team/drill-8, commit a855f3b3, 19/19 test.
- Follow-up F5 (validasi skema + demo canary): commit 634c739f, 21/21 test. Demo: trigger terbukti menyala (canary_log 0 ke 1), restore ditolak exit 1 BACKUP_INVALID menyebut trg_canary_after_insert, md5 database latihan identik sebelum/sesudah.
- Repo notevault: zalana28/drill-7-first-attempt (main 1bd41dab, red-team/drill-8 634c739f).
