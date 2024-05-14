# DuckDB Delta Extension
This is the experimental DuckDB extension for [Delta](https://delta.io/). It is built using the (also experimental) 
[Delta Kernel](https://github.com/delta-incubator/delta-kernel-rs). The extension (currently) offers **read** support for delta
tables, both local and remote.

# Supported platforms
The supported platforms are:
- linux_amd64 and linux_amd64_gcc4
- osx_amd64 and osx_arm64

Support for the [other](https://duckdb.org/docs/extensions/working_with_extensions#platforms) DuckDB platforms is 
work-in-progress

# How to use
**NOTE:** this extension is not yet available, it will be available from DuckDB's upcoming (v0.10.3) release

This extension is distributed as a binary extension. To use it, simply use one of its functions from DuckDB (>= v0.10.3)
and the extension will be autoloaded:
```SQL
FROM delta_scan('s3://some/delta/table');
```

Note that using DuckDB [Secrets](https://duckdb.org/docs/configuration/secrets_manager.html) is supported for authentication:

```SQL
CREATE SECRET (TYPE S3, provider credential_chain);
FROM delta_scan('s3://some/delta/table/with/auth');
```

To scan a local table, use the full path prefixes with `file://`
```SQL
FROM delta_scan('file:///some/path/on/local/machine');
```

# Features
While still experimental, many (scanning) features/optimizations are already supported in this extension as it reuses most of DuckDB's
regular parquet scanning logic:
- multithreaded scans and parquet metadata reading
- data skipping/filter pushdown
  - skipping row-groups in file (based on parquet metadata)
  - skipping complete files (based on delta partition info)
- projection pushdown
- scanning tables with deletion vectors
- all primitive types
- structs

More features coming soon!

# Building
See the [Extension Template](https://github.com/duckdb/extension-template) for generic build instructions
