# GeoIP for EEPs

This project is a fork of [v2fly/geoip](https://github.com/team-cloudchaser/geoip), which releases GeoIP files automatically every Thursday for routing in supported EEPs.

## Differences

- Databases are sourced from [DB-IP.com](https://db-ip.com/db/lite.php) instead of MaxMind.
- Carrier-grade NAT (CNAT) ranges are excluded from `private`, because they should never be considered as such.
- A lite version of the databases are also included covering repressive regimes (Iran, Russia, China, Turkimenistan, Belarus, Egypt).
  - As such, `geoip-only-cn-private.dat` is no longer included.

## Release files
- `geoip.dat`: IP to country database, with private ranges available.
- `asnip.dat`: IP to ASN database with private ranges.
- `geoip-lean.dat`: IP to country database only including countries of interest, with private ranges available.
- `asnip-lean.dat`: IP to ASN database only including countries of interest, with private ranges available.

> The sections below are from the forked project.

## Customizing GeoIP files

### Concepts

These two concepts are notable: `input` and `output`. The `input` is the data source and its input format, whereas the `output` is the destination of the converted data and its output format. What the CLI does is to aggregate all input format data, then convert them to output format and write them to GeoIP files by using the options in the config file.

### Supported formats

Supported `input` formats:

- **cutter**: Remove data from previous steps
- **maxmindGeoLite2CountryCSV**: Convert MaxMind GeoLite2 country CSV data to other formats
- **maxmindMMDB**: Convert MaxMind country mmdb database to other formats
- **private**: Convert LAN and private network CIDR to other formats
- **text**: Convert plaintext IP and CIDR to other formats
- **v2rayGeoIPDat**: Convert V2Ray GeoIP dat to other formats

Supported `output` formats:

- **text**: Convert data to plaintext CIDR format
- **v2rayGeoIPDat**: Convert data to V2Ray GeoIP dat format

### Steps

1. Install `golang` and `git`
2. Clone project code: `git clone https://github.com/v2fly/geoip.git`
3. Navigate to project root directory: `cd geoip`
4. Install project dependencies: `go mod download`
5. Edit config file `config.json` by referencing the configuration options in [configuration.md](https://github.com/v2fly/geoip/blob/HEAD/configuration.md)
6. Generate files: `go run ./`

### Notices

- If input format `maxmindGeoLite2CountryCSV` is specified in config file, you must first download `GeoLite2-Country-CSV.zip` from [MaxMind](https://dev.maxmind.com/geoip/geoip2/geolite2/), then unzip it to `geolite2` directory.
- `go run ./` will use `config.json` in current directory as the default config file, or use `go run ./ -c /path/to/your/own/config/file.json` to specify your own config file.
- The generated files are located at `output` directory by default.
- Run `go run ./ -h` for more usage information.
- See [configuration.md](https://github.com/v2fly/geoip/blob/HEAD/configuration.md) for all configuration options.

## CLI showcase

You can run `go install -v github.com/v2fly/geoip@latest` to install the CLI tool directly.

### Show help information

```bash
$ ./geoip -h
Usage of ./geoip:
  -c string
    	Path to the config file (default "config.json")
  -l	List all available input and output formats
```

### Generate GeoIP files

```bash
$ ./geoip -c config.json
2021/09/02 00:26:12 ✅ [v2rayGeoIPDat] geoip.dat --> output/dat
2021/09/02 00:26:12 ✅ [v2rayGeoIPDat] geoip-only-cn-private.dat --> output/dat
2021/09/02 00:26:12 ✅ [v2rayGeoIPDat] cn.dat --> output/dat
2021/09/02 00:26:12 ✅ [v2rayGeoIPDat] private.dat --> output/dat
2021/09/02 00:26:12 ✅ [v2rayGeoIPDat] test.dat --> output/dat
2021/09/02 00:26:12 ✅ [text] cn.txt --> output/text
```

### List all supported formats

```bash
$ ./geoip -l
All available input formats:
  - cutter (Remove data from previous steps)
  - maxmindGeoLite2CountryCSV (Convert MaxMind GeoLite2 country CSV data to other formats)
  - maxmindMMDB (Convert MaxMind mmdb database to other formats)
  - private (Convert LAN and private network CIDR to other formats)
  - test (Convert specific CIDR to other formats (for test only))
  - text (Convert plaintext IP and CIDR to other formats)
  - v2rayGeoIPDat (Convert V2Ray GeoIP dat to other formats)

All available output formats:
  - text (Convert data to plaintext CIDR format)
  - v2rayGeoIPDat (Convert data to V2Ray GeoIP dat format)
```

## Notice

This product includes GeoLite2 data created by MaxMind, available from [MaxMind](https://www.maxmind.com).

## License

[CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/)
