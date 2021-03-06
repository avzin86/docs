# Creating a registry

{% list tabs %}

- CLI

  {% include [cli-install](../../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

  1. Create a registry:

     ```
     $ yc iot registry create --name my-registry
     
      id: b91ki3851hab9m0l68je
      folder_id: aoek49ghmknnpj1ll45e
      created_at: "2019-05-28T11:29:42.420Z"
      name: my-registry
     ```

     {% include [name-format](../../../_includes/name-format.md) %}

  1. Make sure the registry was created:

      ```
      $ yc iot registry list
       +----------------------+-------------+
       |          ID          |    NAME     |
       +----------------------+-------------+
       | b91ki3851hab9m0l68je | my-registry |
       +----------------------+-------------+
      ```

- Terraform

   {% include [terraform-definition](../../../solutions/_solutions_includes/terraform-definition.md) %}

   If you don't have Terraform, [install it and configure the Yandex.Cloud provider](../../../solutions/infrastructure-management/terraform-quickstart.md#install-terraform).

   {% note info %}

   To add certificates to the registry, [create](../certificates/create-certificates.md) them in advance.

   {% endnote %}

   To create a device registry:

   1. In the configuration file, describe the parameters of the resource to create:
      * `yandex_iot_core_registry`: Registry parameters:
        * `name`: Registry name.
        * `description`: Registry description.
        * `labels`: Registry labels in `key:value` format.
        * `passwords`: List of registry passwords for authorization using a [username and password](../../concepts/authorization.md#log-pass).
        * `certificates`: List of registry certificates for authorization using [certificates](../../concepts/authorization.md#certs).

      Sample resource structure in the configuration file:

      ```
      resource "yandex_iot_core_registry" "my_registry" {
        name        = "test-registry"
        description = "test registry for terraform provider documentation"
        labels = {
          test-label = "label-test"
        }
      
        passwords = [
          "<password 1>",
          "<password 2>"
        ]
      
        certificates = [
          file("<path to the first certificate file>"),
          file("<path to the second certificate file>")
        ]
      }
      
      output "yandex_iot_core_registry_my_registry" {
        value = "${yandex_iot_core_registry.my_registry.id}"
      }
      ```

      For more information about the resources you can create using Terraform, see the [provider documentation](https://www.terraform.io/docs/providers/yandex/index.html).

   2. Make sure that the configuration files are correct.
      1. In the command line, go to the directory where you created the configuration file.
      2. Run the check using the command:

         ```
         $ terraform plan
         ```

      If the configuration is described correctly, the terminal displays a list of created resources and their parameters. If there are errors in the configuration, Terraform points them out.

   3. Deploy the cloud resources.
      1. If the configuration doesn't contain any errors, run the command:

         ```
         $ terraform apply
         ```
      2. Confirm that you want to create the resources.

      Afterwards, all the necessary resources are created in the specified folder. You can check resource availability and their settings in the [management console]({{ link-console-main }}).

{% endlist %}

