# Analisis_Penjualan
Analisis demo penjualan yang dilakukan untuk mendapatkan nilai keuntungan yang tertinggi disuatu daerah, pada penjualan barang dan obat di perusahaan pembuatan obat.

SELECT 
  C.branch_id, C.kota, C.provinsi ,C.branch_name,
  P.product_name,
  T.price, T.discount_percentage, 
  T.price - (T.price*T.discount_percentage) Harga_Setelah_Diskon,
  CASE 
    WHEN T.price <= 50000 THEN 0.10
    WHEN T.price > 50000 AND T.price <= 100000 THEN 0.15
    WHEN T.price > 100000 AND T.price <= 300000 THEN 0.20
    WHEN T.price > 300000 AND T.price <= 500000 THEN 0.25
  ELSE 0.30
  END AS persentase_gross_laba,
  ((T.price - (T.price*T.discount_percentage))* 
  CASE 
    WHEN T.price <= 50000 THEN 0.10
    WHEN T.price > 50000 AND T.price <= 100000 THEN 0.15
    WHEN T.price > 100000 AND T.price <= 300000 THEN 0.20
    WHEN T.price > 300000 AND T.price <= 500000 THEN 0.25
  ELSE 0.30
  END) nett_profit
FROM
  `rakaminkfanalytics-425304.kimia_farma.kf_final_transaction` T
INNER JOIN
  `rakaminkfanalytics-425304.kimia_farma.kf_kantor_cabang` C 
ON 
  T.branch_id = C.branch_id
INNER JOIN
`rakaminkfanalytics-425304.kimia_farma.kf_product` P
ON
  P.product_id = T.product_id
GROUP BY 
  C.branch_id, C.kota, C.provinsi ,C.branch_name,
  P.product_name,
  T.price, T.discount_percentage
ORDER BY nett_profit DESC

DASHBOARD HERE
https://lookerstudio.google.com/reporting/1b4ef73f-44a4-49dd-a9c8-0a4a7553f160
