# Ansible Role: ssmpfs_reader

An Ansible role that reads AWS SSM Parameters create by Terraform module [ssmpfs](https://registry.terraform.io/modules/gibbsoft/ssmpfs/aws/latest).

## Role Variables

Available variables are listed below:

| Variable                    | Default              | Required  | Comments (type)                                           |
| :------------- | :------------------- | :-------: | :--------------------------------------------------------------------- |
| `key`          | n/a         | Y | A key to lookup in SSM Parameter Store                                                  |
| `set_fact`     | n/a         | N | The name of a fact to set with the contents of the SSM param read from `key`            |
| `dest`         | n/a         | N | Destination path of file to populate with the contents of the SSM param read from `key` |

> NB: You can pass either or both `set_fact` and `dest`, but one at least one of these should be used.

## Examples

### 1. Retrieving a chunked file

In the example below, we retrieve a chunked SSM Parameter that was split across three blocks and save the composite result to `outfile.txt`.

The SSM Parameter keys may look like this:

```text
/LoremIpsum/1-of-3
/LoremIpsum/2-of-3
/LoremIpsum/3-of-3
```

The ansible call to the role may look something like this:

```yaml
  roles:
    - role: gibbsoft.ssmpfs_reader
      key: /LoremIpsum
      dest: output.txt
```

The module will discover the *child* SSM Paramter keys and join their contents back together.

### 2. Setting a fact from an SSM parameter

You can set a new fact containing secret retrieved from the SSM Parameter.  In this example we define that `slack_endpoint` should return the recovered value.

```yaml
  roles:
    - role: gibbsoft.ssmpfs_reader
      key: "/demo/slack/endpoint"
      set_fact: slack_endpoint
```

You can make multiple calls to the role as required.

## License

Released under the [GNU General Public License v3](https://github.com/gibbsoft/terraform-module-ssmpfs/blob/main/LICENSE) license.
