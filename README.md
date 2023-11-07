# st_metadata_parser
a light-weight metadata parser for safetensors files

```python
import json
import os
import sys
from pprint import pprint


def safetensors_metadata_parser(file_path):
    header_size = 8
    meta_data = {}
    if os.stat(file_path).st_size > header_size:
        with open(file_path, "rb") as f:
            b8 = f.read(header_size)
            if len(b8) == header_size:
                header_len = int.from_bytes(b8, 'little', signed=False)
                headers = f.read(header_len)
                if len(headers) == header_len:
                    meta_data = sorted(json.loads(headers.decode("utf-8")).get("__metadata__", meta_data).items())
    return meta_data


def main(argv):
    for filename in argv:
        metadata = safetensors_metadata_parser(filename)
        print(f"{filename}'s metadata:")
        pprint(metadata)


if __name__ == "__main__":
    main(sys.argv[1:])
```

command like:

        python st_metadata_parser.py input_file.safetensors
or

        python st_metadata_parser.py input_file1.safetensors input_file2.safetensors ... input_fileN.safetensors
