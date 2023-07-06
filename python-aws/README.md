# Python and AWS interaction

# S3 buckets with boto3

## List objects in a bucket

First we need to have credentials and a profile in `.aws/credentials`.

Then, prepare the aws ressources and access them using the appropriate profile:

```python
s3_profile = 'a_profile'
bucket_name = 'the-bucket'

boto3.setup_default_session(profile_name=s3_profile)
s3 = boto3.resource('s3')
bucket = s3.Bucket(bucket_name)
```

A utility that uses the `bucket` object to list either folders or some types of files:

```python
def list_objects_in_s3_folder(
    s3_path,
    bucket,
    return_type='folder'
):
    """
    Lists the items in s3 contained in
    target folder `s3_path` filtered by
    the `return_type`.

    Args:
        s3_path (str): the path to the target folder,
            relative to the root of the s3 bucket

        bucket (boto3.resource.Bucket): the
            Bucket instance to interact with.

        return_type (str, optional): the type of
            files that we want to filter out in the
            returned list. Defaults to 'folder'.

    Raises:
        Exception: when the return_type
        is not implemented.

    Returns:
        list: the list of files.
    """
    the_list = [
        i.key
        for i in bucket.objects.filter(Prefix=s3_path)
    ]
    valid_return_file_types = [
        '.parquet',
        '.json'
    ]
    if return_type == 'folder':
        folders_found = []
        for item in the_list:
            if item.endswith('/'):
                s = item[:-1].rsplit('/', 1)
                folder_name = s[-1]
                prefix = s[0]
                if (
                    prefix == s3_path
                    and folder_name != s3_path
                    and folder_name != ''
                ):
                    folders_found.append(folder_name)
        the_list = folders_found
    elif return_type in valid_return_file_types:
        files_found = []
        for item in the_list:
            if not item.endswith('/'):
                s = item.rsplit('/', 1)
                file_name = s[-1]
                prefix = s[0]
                if (
                    prefix == s3_path
                    and file_name != s3_path
                    and file_name != ''
                    and '/' not in file_name
                    and file_name.endswith(return_type)
                ):
                    files_found.append(file_name)
        the_list = files_found
    else:
        msg = f'Unsupported return_type {return_type}'
        raise Exception(msg)
    return the_list
```

## Download an object

Once we have the `bucket` instance,

```python
bucket.download_file(s3_path, local_destination_path)
```

***

Return to **[main page](../README.md)** 
