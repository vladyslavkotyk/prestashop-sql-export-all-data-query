
## It wont work on prestashop sql manager, need to use a tool like phpmyadmin


```sql
SELECT
p.active 'Active',
m.name 'Manufacturer',
p.id_product 'ID Product',
p.reference 'Product eference',
pl.name 'Product name',
GROUP_CONCAT(DISTINCT(al.name) SEPARATOR ", ") AS 'Combination',
pa.id_product_attribute 'ID product attribute',
pa.reference 'Attribute reference',
pa.supplier_reference 'Attribute supplier reference',
s.quantity 'Quantity',
p.price 'Price w/o VAT',
pa.price 'Combination price',
p.wholesale_price 'Wholesale price',
GROUP_CONCAT(DISTINCT(cl.name) SEPARATOR ",") AS 'Product groups',
p.weight 'Weight',
p.id_tax_rules_group 'TAX group',
pa.reference 'Combination reference',
pl.description_short 'Short description',
pl.description 'Long description',
pl.meta_title 'Meta Title',
pl.meta_keywords 'Meta Keywords',
pl.meta_description 'Meta Description',
pl.link_rewrite 'Link',
pl.available_now 'In stock text',
pl.available_later 'Coming text',
p.available_for_order 'Orderable text',
p.date_add 'Added',
p.show_price 'Show price',
p.online_only 'Only online',
GROUP_CONCAT(DISTINCT CONCAT(
        '/img/p/', 
        IF(CHAR_LENGTH(im.id_image) >= 5, CONCAT(SUBSTRING(im.id_image, -5, 1), '/'), ''),
        IF(CHAR_LENGTH(im.id_image) >= 4, CONCAT(SUBSTRING(im.id_image, -4, 1), '/'), ''),
        IF(CHAR_LENGTH(im.id_image) >= 3, CONCAT(SUBSTRING(im.id_image, -3, 1), '/'), ''),
        IF(CHAR_LENGTH(im.id_image) >= 2, CONCAT(SUBSTRING(im.id_image, -2, 1), '/'), ''),
        IF(CHAR_LENGTH(im.id_image) >= 1, CONCAT(SUBSTRING(im.id_image, -1, 1), '/'), ''),
        im.id_image,
    '.jpg'), ", ") AS 'Images (x,y,z...)'
FROM
psi4_product p
LEFT JOIN psi4_product_lang pl ON (p.id_product = pl.id_product and pl.id_lang=2)
LEFT JOIN psi4_manufacturer m ON (p.id_manufacturer = m.id_manufacturer)
LEFT JOIN psi4_category_product cp ON (p.id_product = cp.id_product)
LEFT JOIN psi4_category c ON (cp.id_category = c.id_category)
LEFT JOIN psi4_category_lang cl ON (cp.id_category = cl.id_category and cl.id_lang=2)
LEFT JOIN psi4_product_attribute pa ON (p.id_product = pa.id_product)
LEFT JOIN psi4_stock_available s ON (p.id_product = s.id_product and (pa.id_product_attribute=s.id_product_attribute or pa.id_product_attribute is null))
LEFT JOIN psi4_product_tag pt ON (p.id_product = pt.id_product)
LEFT JOIN psi4_product_attribute_combination pac ON (pac.id_product_attribute = pa.id_product_attribute)
LEFT JOIN psi4_attribute_lang al ON (al.id_attribute = pac.id_attribute and al.id_lang=2)
LEFT JOIN psi4_shop sh ON p.id_shop_default = sh.id_shop 
LEFT JOIN psi4_shop_url su ON su.id_shop = sh.id_shop AND su.main = 1
LEFT JOIN psi4_image im ON (p.id_product = im.id_product AND im.cover = 1)
LEFT JOIN psi4_product_attribute_image pai ON (pai.id_product_attribute = s.id_product_attribute)
LEFT JOIN psi4_configuration conf ON conf.name = 'psi4_SHOP_DOMAIN'
GROUP BY p.id_product,pac.id_product_attribute order by p.id_product;

```
