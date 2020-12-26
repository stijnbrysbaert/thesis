Describes the capacity and rental availability of a station. Data returned should be as close to realtime as possible, but in no case should it be more than 5 minutes out-of-date.  See [Data Latency](#data-latentcy). Data reflects the operators most recent knowledge of the station’s status. Any station that is represented in `station_status.json` must have a corresponding entry in `station_information.json`.

Field Name | Required | Type | Defines
---|---|---|---
`stations` | Yes | Array | Array that contains one object per station in the system as defined below.
\-&nbsp;`station_id` | Yes | ID | Identifier of a station see [station_information.json](#station_informationjson).
\-&nbsp;`num_bikes_available` | Yes | Non-negative integer | Number of functional vehicles physically at the station that may be offered for rental. To know if the vehicles are available for rental, see `is_renting`. <br/><br/>If `is_renting` = `true` this is the number of vehicles that are currently available for rent. If `is_renting` =`false` this is the number of vehicles that would be available for rent if the station were set to allow rentals.
\- `vehicle_types_available` <br/>*(added in v2.1-RC)* | Conditionally Required | Array | This field is required if the [vehicle_types.json](#vehicle_typesjson) file has been defined. This field's value is an array of objects. Each of these objects is used to model the total number of each defined vehicle type available at a station. The total number of vehicles from each of these objects should add up to match the value specified in the `num_bikes_available`  field.
&emsp;\- `vehicle_type_id` <br/>*(added in v2.1-RC)* | Yes | ID | The `vehicle_type_id` of each vehicle type at the station as described in [vehicle_types.json](#vehicle_typesjson). This field is required if the [vehicle_types.json](#vehicle_typesjson) is defined.
&emsp;\- `count` <br/>*(added in v2.1-RC)* | Yes | Non-negative integer | A number representing the total number of available vehicles of the corresponding `vehicle_type_id` as defined in [vehicle_types.json](#vehicle_typesjson) at the station.
\-&nbsp;`num_bikes_disabled` | Optional | Non-negative integer | Number of disabled vehicles of any type at the station. Vendors who do not want to publicize the number of disabled vehicles or docks in their system can opt to omit station capacity (in station_information), `num_bikes_disabled` and `num_docks_disabled` *(as of v2.0)*. If station capacity is published, then broken docks/vehicles can be inferred (though not specifically whether the decreased capacity is a broken vehicle or dock).
\-&nbsp;`num_docks_available` | Conditionally required <br/>*(as of v2.0)* | Non-negative integer | Required except for stations that have unlimited docking capacity (e.g. virtual stations) *(as of v2.0)*. Number of functional docks physically at the station that are able to accept vehicles for return. To know if the docks are accepting vehicle returns, see `is_returning`. <br /><br/> If `is_returning` = `true` this is the number of docks that are currently available to accept vehicle returns. If `is_returning` = `false` this is the number of docks that would be available if the station were set to allow returns. 
\- `vehicle_docks_available` <br/>*(added in v2.1-RC)* | Conditionally Required | Array | This field is required in feeds where the [vehicle_types.json](#vehicle_typesjson) is defined and where certain docks are only able to accept certain vehicle types. If every dock at the station is able to accept any vehicle type, then this field is not required. This field's value is an array of objects. Each of these objects is used to model the number of docks available for certain vehicle types. The total number of docks from each of these objects should add up to match the value specified in the `num_docks_available` field.
&emsp;\- `vehicle_type_ids` <br/>*(added in v2.1-RC)* | Yes | Array of Strings | An array of strings where each string represents a vehicle_type_id that is able to use a particular type of dock at the station
&emsp;\- `count` <br/>*(added in v2.1-RC)* | Yes | Non-negative integer | A number representing the total number of available vehicles of the corresponding vehicle type as defined in the `vehicle_types` array at the station that can accept vehicles of the specified types in the `vehicle_types` array.
\-&nbsp;`num_docks_disabled` | Optional | Non-negative integer | Number of disabled dock points at the station.
\-&nbsp;`is_installed` | Yes | Boolean | Is the station currently on the street?<br/><br/>`true` - Station is installed on the street.<br/>`false` - Station is not installed on the street.<br/><br/>Boolean should be set to `true` when equipment is present on the street. In seasonal systems where equipment is removed during winter, boolean should be set to `false` during the off season. May also be set to false to indicate planned (future) stations which have not yet been installed.
\-&nbsp;`is_renting` | Yes | Boolean | Is the station currently renting vehicles? <br /><br />`true` - Station is renting vehicles. Even if the station is empty, if it would otherwise allow rentals this value must be `true`.<br/>`false` - Station is not renting vehicles.<br/><br/>If the station is temporarily taken out of service and not allowing rentals this field must be set to `false`.<br/><br/>If a station becomes inaccessible to users due to road construction or other factors this field should be set to `false`. Field should be set to `false` during hours or days when the system is not offering vehicles for rent.
\-&nbsp;`is_returning` | Yes | Boolean | Is the station accepting vehicle returns? <br /><br />`true` - Station is accepting vehicle returns. Even if the station is full, if it would otherwise allow vehicle returns this value must be `true`.<br /> `false` - Station is not accepting vehicle returns.<br/><br/>If the station is temporarily taken out of service and not allowing vehicle returns this field must be set to `false`.<br/><br/>If a station becomes inaccessible to users due to road construction or other factors this field should be set to `false`.
\-&nbsp;`last_reported` | Yes | Timestamp | The last time this station reported its status to the operator's backend. 


#### Example:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "3.0",
  "data": {
    "stations": [
      {
        "station_id": "station 1",
        "is_installed": true,
        "is_renting": true,
        "is_returning": true,
        "last_reported": 1434054678,
        "num_docks_available": 3,
        "vehicle_docks_available": [{
          "vehicle_type_ids": ["abc123"],
          "count": 2
        }, {
          "vehicle_type_ids": ["def456"],
          "count": 1
        }],
        "num_bikes_available": 1,
        "vehicle_types_available": [{
          "vehicle_type_id": "abc123",
          "count": 1
        }, {
          "vehicle_type_id": "def456",
          "count": 0
        }]        
      }, {
        "station_id": "station 2",
        "is_installed": true,
        "is_renting": true,
        "is_returning": true,
        "last_reported": 1434054678,
        "num_docks_available": 8,
        "vehicle_docks_available": [{
          "vehicle_type_ids": ["abc123"],
          "count": 6
        }, {
          "vehicle_type_ids": ["def456"],
          "count": 2
        }],
        "num_bikes_available": 6,
        "vehicle_types_available": [{
          "vehicle_type_id": "abc123",
          "count": 2
        }, {
          "vehicle_type_id": "def456",
          "count": 4
        }]
      }
    ]
  }
}
```