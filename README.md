# SQL-Foundation-Studi-Independent

**I got the dataset from Bigquery Public Dataset:
theLook eCommerce https://console.cloud.google.com/marketplace/product/bigquery-public-data/thelook-ecommerce**
```
SELECT * FROM `bigquery-public-data.new_york_trees.tree_species` 
```

#### 1 Keragaman Spesies Pohon 
Insight: Dataset ini mencakup berbagai jenis pohon dari segala jenis lingkungan. 
```
SELECT 
species_scientific_name, 
species_common_name
FROM `bigquery-public-data.new_york_trees.tree_species` 
ORDER BY species_scientific_name ASC
```

#### 2 Pertumbuhan Cepat vs Pertumbuhan Lama
Insight: Menemukan perbandingan antara pohon yang tumbuh cepat dan pohon yang tumbuh lambat dapat memberikan pandangan tentang pilihan tanaman yang sesuai dengan kebutuhan waktu.
```
SELECT 
  growth_rate,
  COUNT(DISTINCT species_common_name) AS total_trees,
  STRING_AGG(DISTINCT species_common_name, '-') AS list_of_tree
FROM 
  `bigquery-public-data.new_york_trees.tree_species`
GROUP BY 
  growth_rate
ORDER BY 
  total_trees ASC
```

#### 3 Warna Daun Musim Gugur yang Menarik:
Insight: Identifikasi pohon dengan warna daun musim gugur yang paling mencolok, memberikan keindahan yang berkelanjutan sepanjang tahun.
```
SELECT 
  fall_color,
  COUNT(DISTINCT species_common_name) AS total_trees,
  STRING_AGG(DISTINCT species_common_name, '-') AS list_of_tree
FROM 
  `bigquery-public-data.new_york_trees.tree_species`
GROUP BY 
  fall_color
ORDER BY 
  total_trees ASC
```

#### 4 Toleransi Lingkungan dan Lokasi:
Insight: Menemukan pohon yang toleran terhadap lingkungan dan lokasi tertentu membantu dalam perencanaan taman yang lebih efektif.
```
SELECT 
species_common_name,
environmental_tolerances,
location_tolerances
FROM 
  `bigquery-public-data.new_york_trees.tree_species`
WHERE 
location_tolerances LIKE '%Narrow Growing Space%' 
AND location_tolerances LIKE '%Small Tree Pit (<3 ft)%'
```


#### 5 Cultivar Terpopuler:
Insight: Mengenali cultivar yang populer dapat memberikan petunjuk bagi pemilik kebun yang mencari variasi yang terbukti berhasil.
```
SELECT 
species_common_name,
notes_suggested_cultivars
FROM 
`bigquery-public-data.new_york_trees.tree_species`
```


#### 6 Ukuran Pohon dan Penempatannya:
Insight: Pohon yang sesuai dengan ukuran tertentu dapat direkomendasikan untuk area tertentu, memberikan panduan dalam perencanaan tata letak taman.
```
SELECT 
species_common_name,
tree_size,
location_tolerances
FROM 
`bigquery-public-data.new_york_trees.tree_species`
```

#### 7 Bentuk dari pohon dan apa saja yang terbanyak
insight : pohon yang memiliki bentuk paling banyak adalah bentuk yang paling populer saat ini
```
SELECT 
  form,
  COUNT(DISTINCT species_common_name) AS total_trees,
  STRING_AGG(DISTINCT species_common_name, '-') AS list_of_tree
FROM 
  `bigquery-public-data.new_york_trees.tree_species`
GROUP BY 
  form
ORDER BY 
  total_trees ASC
```

#### 8 Rekomendasi spesifik tentang lokasi wilayah yang dapat ditanami pohon. 
Insight: Pemahaman mendalam tentang cara terbaik menanam dan merawat pohon tertentu dapat memberikan nilai tambah bagi pemilik kebun.
```
SELECT 
  'Wilayah Tinggi Polutan' AS jenis_wilayah,
  STRING_AGG(DISTINCT 
    CASE WHEN environmental_tolerances LIKE '%Pollution%' THEN species_common_name END, '-') AS list_pohon
FROM 
  `bigquery-public-data.new_york_trees.tree_species`
GROUP BY 
  jenis_wilayah

UNION ALL

SELECT 
  'Wilayah Pesisir Pantai' AS jenis_wilayah,
  STRING_AGG(DISTINCT 
    CASE WHEN environmental_tolerances LIKE '%High pH Tolerant%' THEN species_common_name END, '-') AS list_pohon
FROM 
  `bigquery-public-data.new_york_trees.tree_species`
GROUP BY 
  jenis_wilayah
```

#### 9 Korelasi antara Pertumbuhan dan Toleransi Lingkungan:
Insight: Menemukan hubungan antara pertumbuhan cepat dan tingkat toleransi lingkungan dapat membantu pemilik taman memilih pohon yang sesuai dengan kondisi tanah dan cuaca setempat.
```
SELECT 
  growth_rate,
  STRING_AGG(species_common_name, ',') AS list_tree,
  STRING_AGG(DISTINCT environmental_tolerances, ', ') AS  environmental
FROM 
  `bigquery-public-data.new_york_trees.tree_species`
GROUP BY 
  growth_rate
```

#### 10 Mendapatkan rekomendasi tempat untuk menanam pohon
Insight : Beberapa informasi mengenai pohon dan saran untuk menanam pohon tersebut
```
SELECT 
  species_common_name,
  environmental_tolerances,
  location_tolerances,
  comments

FROM 
  `bigquery-public-data.new_york_trees.tree_species`
WHERE 
IFNULL(comments, '') != ''
```




