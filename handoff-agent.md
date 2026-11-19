Handoff: notevault (proyek latihan Express + SQLite, drill #7-#8)

State [F]: repo zalana28/drill-7-first-attempt. Main: commit 1bd41dab (drill #7, 10 test). Branch red-team/drill-8: commit 634c739f terbaru — berisi fix F1-F5, 21/21 test lolos. File kunci: scripts/restore.js (verifyDbFile), scripts/demo-canary-restore.js, tests/redteam.test.js. Branch belum di-merge ke main; jangan merge tanpa diminta.

Terverifikasi [F]: F2 — restore lama menimpa database sebelum verifikasi; sudah diperbaiki (verifikasi dulu, BACKUP_INVALID, rollback otomatis). F5 — backup berisi trigger apa pun ditolak; demo canary membuktikannya (canary_log 0 ke 1, restore exit 1, md5 database identik). Test 21/21, commit 634c739f, 4 blob terverifikasi dari GitHub.
Belum terverifikasi [I]: tidak ada deployment produksi; tidak ada pengguna; performa di luar sandbox tidak diukur.

Koreksi brief: tidak ada insiden produksi dan tidak ada data hilang permanen — F2 ditemukan di red-team drill atas database latihan. Jangan warisi narasi brief; pakai record di atas.

Standing orders: bounty watcher make.fun tetap jalan (cron makefun-board-watch tiap 6 jam); bot Polyester STOP atas perintah zalana — jangan nyalakan lagi; jangan kerjakan fix cancelDelay drill #9 dulu (backlog opsional).

3 pitfall dari sejarahku:
1. Tool baca GitHub mengabaikan parameter branch — baca branch non-default pakai ref.
2. Whitelist tabel saja tidak cukup: trigger yang menempel di tabel notes lolos — tolak trigger apa pun.
3. Cek server jangan pakai HTTP /health — listener hang tidak menjawab; pakai raw TCP connect.

Next action: tanyakan ke zalana/Hermes apakah branch red-team/drill-8 boleh di-merge ke main; sampai ada jawaban, jangan merge dan jangan ubah artefak yang sudah dinilai.
