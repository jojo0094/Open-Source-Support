# Introduction
InfoAsset Manager has an asset-centric relational object structure, in most instances assets have a relationship based on the Object ID to multiple other objects (asset-asset [Node-Pipe, Pipe-Property], asset-survey [Pipe-CCTV Survey] etc.).  
Using InfoAsset Manager Executive Suite it is possible to also define custom relationships, using a User_Text_n field to relate to the Object ID of the related object.  
By the script examples in this repository relationships are used against any two fields.  

## [UI-UpdateBlockagePropertyID.rb](./UI-UpdateBlockagePropertyID.rb)
Query: Is it possible to look-up the property ID within the Property grid based upon the location and assign it to the property ID in the Blockage incident with a query?  
Request to Update the property_id field of a Blockage Incident with the Property ID form the Property table by comparing the location field on the Blockage Incident to the property_address on the Property object.  

## [UI-UpdateObjectFromObject_ByPrompt_3.rb](./UI-UpdateObjectFromObject_ByPrompt_3.rb)
This script essentially runs the same action as UI-UpdateBlockagePropertyID.rb but at runtime a prompt dialog is displayed to the user to enter the source & destination fields for the comparison and which fields to extract from & update to.  
Options for Source & Destination are: Tables, Fields for comparison, Fields to pull from/to.  

V2 Adds in support for overwriting existing values in destination.  
V3 Adds in support for Flags.  

## [UI-CountConnections.rb](./UI-CountConnections.rb)
In the user's scenario, each Connection Pipe has the Asset_ID of the related Pipe it connects to.  Count the number of Connection Pipes per Pipe and write the count to the User_Text_5 field.  

## [UI-CountRepairs.rb](./UI-CountRepairs.rb)
In the user's scenario, each Pipe Repair has a classification in User_Text_8 and the Asset ID of the related Pipe in User_Text_10. Count the number of 'reactive network' repairs to the User_Text_6 of the Pipe object.  

## [UI-CopyGeneralSurveyAttachmentsToProperty.rb](./UI-CopyGeneralSurveyAttachmentsToProperty.rb)
Copies all attachment details from General Survey (`cams_general_survey`) objects to their associated Property (`cams_property`) object.  
The match is made by comparing the survey's `asset_id` field to the property's `id`.  
All four attachment fields - `purpose`, `filename`, `description`, and `db_ref` - are copied.  
Duplicate detection is performed using the `db_ref` value: if an attachment with the same `db_ref` already exists on the property, it is skipped, making the script safe to re-run.  
