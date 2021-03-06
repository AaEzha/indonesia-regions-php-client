# Indonesia Regions PHP Client

<p align="center">
    <a href="https://travis-ci.com/github/dekiakbar/indonesia-regions-php-client"><img src="https://travis-ci.com/dekiakbar/indonesia-regions-php-client.svg?branch=master" alt="Build Status"></a>
    <a href="https://packagist.org/packages/dekiakbar/indonesia-regions-php-client"><img src="https://poser.pugx.org/dekiakbar/indonesia-regions-php-client/version" alt="Build Status"></a>
    <a href="https://packagist.org/packages/dekiakbar/indonesia-regions-php-client"><img src="https://poser.pugx.org/dekiakbar/indonesia-regions-php-client/license" alt="Build Status"></a>
</p>

---

## About Indonesia Regions PHP Client

> This package can be used to retrieve provinces, cities, districts and villages data from [BPS]( https://bps.go.id).
**Note:** Wherever you use this package please specify [BPS]( https://bps.go.id) as the data source.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [License](#license)

---

## Prerequisites

Before you install this package, make sure that the requirements below are met

```
PHP >= 7.1
ext-json
ext-curl
```

---

## Installation

- `$ composer require dekiakbar/indonesia-regions-php-client`
- And You're done.

---

## Usage

- You can pass the parameter to the method, All of this package has 3 mode :
- For **BPS** and **POS** mode you can use **BPS Id/Code** to retrieve province / city / subdistrict / village data. But you **Can Not** use **BPS Id/Code** to retrieve province / city / subdistrict / village data if you use **dagri** mode, please use **dagri Id/code** to retrieve province / city / subdistrict / village data. 
- If mode is **NULL**, then the method will use default mode, default mode is **bps**.
- **bps** : only return : **kode** and **nama** of province / city / subdistrict / village.
- **dagri** : return : **kode_bps**, **nama_bps**, **kode_dagri**, **nama_dagri** of province / city / subdistrict / village.
- **pos** : return : **kode_bps**, **nama_bps**, **kode_pos**, **nama_pos** of province / city / subdistrict / village.

## Note
- **Not all method always return `list` and `detail` object, on some method only return `list` or `detail` you must check it again before you use the response**.
- If you just need the data (not planing to use this API), just import the Database Dump on `export` folder, extract `indonesia_regions_pos.tar.gz` and import to your database. 
- If you want to retrieve data with mode **Dagri** please fill **Parent Id** (Provinde Id / City Id / Subdistrict Id) with valid data. **Parent Id for POS and Dagri are different**. You can get the city Id `(kode)` from `$region->getCityListByProvinceId('pos',$provinceId)->list`.
- please remeber if you want fetch any data you must use parent Id (province Id / City Id / Subdistrict Id) from  **list** collection, **NOT** from detail collection **(If you use ID from detail it will work but you will get duplicate data)**.

- #### Retrieve All province data

    ```php
    require_once __DIR__ . '/vendor/autoload.php';
    use Dekiakbar\IndonesiaRegionsPhpClient\Region;
    $region = new Region();
    print_r( $region->getAllProvince('pos') );

    // will return something like :
    stdClass Object
    (
        [detail] => Array
            (
                [0] => stdClass Object
                    (
                        [kode_bps] => 11
                        [nama_bps] => ACEH
                        [kode_pos] => 20000
                        [nama_pos] => ACEH
                    )

                [1] => stdClass Object
                    (
                        [kode_bps] => 51
                        [nama_bps] => BALI
                        [kode_pos] => 80000
                        [nama_pos] => BALI
                    )
            )
        [list] => Array
            (
                [0] => stdClass Object
                    (
                        [kode] => 11
                        [nama] => ACEH
                    )

                [1] => stdClass Object
                    (
                        [kode] => 12
                        [nama] => SUMATERA UTARA
                    )
            )
    )

    ```

- #### Retrieve City List By Province Id

    ```php
    require_once __DIR__ . '/vendor/autoload.php';
    use Dekiakbar\IndonesiaRegionsPhpClient\Region;
    $region = new Region();

    // province Id from $region->getAllProvince('bps')
    $provinceId = 32;
    print_r( 
        $region->getCityListByProvinceId('pos',$provinceId) 
    );

    // will return something like :
    stdClass Object
    (
        [detail] => Array
            (
                [0] => stdClass Object
                    (
                        [kode_bps] => 3204
                        [nama_bps] => BANDUNG
                        [kode_pos] => 40300
                        [nama_pos] => BANDUNG
                    )

                [1] => stdClass Object
                    (
                        [kode_bps] => 3273
                        [nama_bps] => BANDUNG
                        [kode_pos] => 40100
                        [nama_pos] => KOTA BANDUNG
                    )
            )
        [list] => Array
            (
                [0] => stdClass Object
                    (
                        [kode] => 3201
                        [nama] => BOGOR
                    )

                [1] => stdClass Object
                    (
                        [kode] => 3202
                        [nama] => SUKABUMI
                    )
            )
    )
    ```

- #### Retrieve Subdistrict List By City Id

    ```php
    require_once __DIR__ . '/vendor/autoload.php';
    use Dekiakbar\IndonesiaRegionsPhpClient\Region;
    $region = new Region();

    // POS
    // city Id from $region->getCityListByProvinceId('pos',$provinceId)->list
    $cityId = 3273;
    print_r( 
        $region->getSubdistrictListByCityId('pos',$cityId)
    );

    // will return something like :
    stdClass Object
    (
        [detail] => Array
            (
                [0] => stdClass Object
                    (
                        [kode_bps] => 3273180
                        [nama_bps] => ANDIR
                        [kode_pos] => 40181
                        [nama_pos] => Andir
                    )

                [1] => stdClass Object
                    (
                        [kode_bps] => 3273180
                        [nama_bps] => ANDIR
                        [kode_pos] => 40182
                        [nama_pos] => Andir
                    )
            )
        [list] => Array
            (
                [0] => stdClass Object
                    (
                        [kode] => 3273010
                        [nama] => BANDUNG KULON
                    )

                [1] => stdClass Object
                    (
                        [kode] => 3273020
                        [nama] => BABAKAN CIPARAY
                    )
            )
    )



    // Dagri
    // city Id from $region->getCityListByProvinceId('dagri',$provinceId)->list
    $cityId = '32.73';
    print_r(
        $region->getSubdistrictListByCityId('dagri',$cityId)
    );

    // will return something like :
    stdClass Object
    (
        [detail] => Array
            (
                [0] => stdClass Object
                    (
                        [kode_bps] => 3273050
                        [nama_bps] => ASTANAANYAR
                        [kode_dagri] => 32.73.10
                        [nama_dagri] => ASTANA ANYAR
                    )

                [1] => stdClass Object
                    (
                        [kode_bps] => 3273120
                        [nama_bps] => UJUNG BERUNG
                        [kode_dagri] => 32.73.26
                        [nama_dagri] => UJUNGBERUNG
                    )
            )
        [list] => Array
            (
                [0] => stdClass Object
                    (
                        [kode] => 32.73.01
                        [nama] => Sukasari
                    )

                [1] => stdClass Object
                    (
                        [kode] => 32.73.02
                        [nama] => Coblong
                    )
            )
    )
    ```

- #### Retrieve Village List By Subdistrict Id

    ```php
    require_once __DIR__ . '/vendor/autoload.php';
    use Dekiakbar\IndonesiaRegionsPhpClient\Region;
    $region = new Region();
    // POS
    // subdistrict Id from $region->getSubdistrictListByCityId('pos',$cityId)->list
    $subdistrictId = 3273010;
    print_r( 
        $region->getVillageListBySubdistrictId('pos',$subdistrictId)
    );

    // will return something like :
    stdClass Object
    (
        [detail] => Array
            (
                [0] => stdClass Object
                    (
                        [kode_bps] => 3273010005
                        [nama_bps] => CARINGIN
                        [kode_pos] => 40212
                        [nama_pos] => Caringin
                    )

                [1] => stdClass Object
                    (
                        [kode_bps] => 3273010007
                        [nama_bps] => CIBUNTU
                        [kode_pos] => 40212
                        [nama_pos] => Cibuntu
                    )
            )
        [list] => Array
            (
                [0] => stdClass Object
                    (
                        [kode] => 3273010001
                        [nama] => GEMPOL SARI
                    )

                [1] => stdClass Object
                    (
                        [kode] => 3273010002
                        [nama] => CIGONDEWAH KALER
                    )
            )
    )

    // Dagri
    // subdistrict Id from $region->getSubdistrictListByCityId('dagri',$cityId)->list
    $subdistrictId = '32.73.10';
    print_r( 
        $region->getVillageListBySubdistrictId('dagri',$subdistrictId) 
    );

    // will return something like :
    stdClass Object
    (
        [detail] => Array
            (
                [0] => stdClass Object
                    (
                        [kode_bps] => 3273050001
                        [nama_bps] => KARASAK
                        [kode_dagri] => 32.73.10.1001
                        [nama_dagri] => KARASAK
                    )

                [1] => stdClass Object
                    (
                        [kode_bps] => 3273050002
                        [nama_bps] => PELINDUNG HEWAN
                        [kode_dagri] => 32.73.10.1006
                        [nama_dagri] => PELINDUNG HEWAN
                    )
            )
    )

    ```

- #### Retrieve ISO 3166-2 code for province
    > **Note : This method only for province**
    ```php
    require_once __DIR__ . '/vendor/autoload.php';
    use Dekiakbar\IndonesiaRegionsPhpClient\Region;
    $region = new Region();

    // Province code get from $region->getAllProvince('bps')->list
    $provinceId = 32;
    print_r( 
            $region->getIsoCode($provinceId) 
    );

    // will return String, something like :
    ID-JB

    ```
---

## License

[![License](https://poser.pugx.org/dekiakbar/indonesia-regions-php-client/license)](//packagist.org/packages/dekiakbar/indonesia-regions-php-client)

- **[MIT license](http://opensource.org/licenses/mit-license.php)**
- Copyright 2020 © <a href="https://github.com/dekiakbar" target="_blank">Deki Akbar</a>.