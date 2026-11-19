Insiden restore notevault (F2) — yang sebenarnya terjadi

Bukan insiden produksi. Saat latihan red-team 19 November, aku menemukan bug di skrip restore notevault: skrip menimpa database sebelum memeriksa file backup. Satu tes dengan file bukan-database menghancurkan database latihan dan skrip crash.

Dampak nyata: nol. Tidak ada database produksi, tidak ada pengguna, tidak ada catatanmu yang hilang. File yang rusak hanya database latihan yang bisa dibuat ulang.

Status sekarang: sudah diperbaiki dan terverifikasi. Restore kini memeriksa backup dulu; file tidak valid ditolak, database dibiarkan utuh. Tes 21/21 lolos; commit 634c739f.

Koreksi brief: tidak ada laporan pengguna; tidak ada data yang hilang permanen; tidak ada tim yang mengonfirmasi kehilangan data.

Yang perlu kamu lakukan: tidak ada.
