# Extract API

Access your entities via a securable HTTP API in a clean JSON format.

## Client modules may implement per-entity, per-bundle hooks.

Your modules may implement HOOK_extract_api_ENTITYTYPE__BUNDLE_callbacks_alter(&$callbacks).

This hook specifies property processor callbacks and the fields which they should process.

## Example hook

{{{
/**
 * Implements HOOK_extract_api_file__image_callbacks_alter()
 *  Type: file
 *  Bundle: image
 */
function my_module_extract_api_file__image_callbacks_alter(&$callbacks) {
  // This callback can be used for quick inspection of the objects during API buildout.
  // $callbacks['entity'] = array(
  //   'entity',
  // );

  $callbacks['field_date'] = array(
    'field_my_module_date',
  );
  $callbacks['property'] = array(
    'status',
    'uri',
    'alt',
    'title',
    'metadata',
    'metatags',
  );
}
}}}

## Available callback methods

* entity
    * This processor is provided for convenience when building and debugging.
* property
    * Processes fields designated as native entity object properties.
* field_date
    * Processes date api_fields into a non-lossy format.
* wrapper_property
    * The fallback. Unless a property is specified in another 

## Example hook

## API calls 
 
 API call pattern:
 
 https://yourdomain.com/api/extract/file/image?api_key=%API_KEY_HERE%&force_expire=0
 
 Example API call:
 
 https://yourdomain.com/api/extract/file/image?api_key=01030c2c59efdf2f32d3721ce1895b4e&force_expire=0
 
## API key

To generate an API key hash from command line:

{{{
echo -n "df2f32d3721ce1895b4e01030c2c59ef" | shasum -a512
}}}

Then set that hash in settings.php to provide access.

{{{
$conf['extract_api_key_hashes'] = array(
    '801010f71745470e56fd...', // Good Bob's API key,
    // 'd801010454f71440e56f...', // Evil Bob's API key,
);
}}}