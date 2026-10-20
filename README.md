# Distribution, matching GymM8

- **Private source:** `Sajma27/GoalTracker`.
- **Public iPhone downloads:** `Sajma27/GoalTracker-AltStore`, containing only source metadata, icon and IPA releases.
- **Garmin:** a separate private Connect IQ beta of type **Data Field**, targeting Forerunner 970.

Build the phone IPA with `Scripts/build-altstore-ipa.sh`. It builds device code without an Apple provisioning profile, ad-hoc signs the Garmin framework and app, verifies signatures, and packages `Payload/GoalTracker.app`. AltStore Classic re-signs the app for the user's Apple ID, as with GymM8.

Generate exact metadata after building:

```sh
python3 Scripts/prepare-altstore-source.py build/altstore/GoalTracker-1.0-1.ipa --date 2026-10-21
```

The output `build/distribution/source.json` contains the exact IPA size and SHA-256. Upload the IPA to release `v1.0-1` in the distribution repository, then commit the generated source and icon there. Verify that its download URL resolves to that release's actual bytes. A pre-release is appropriate until physical-device validation is complete.

Add this source in AltStore Classic:

`https://raw.githubusercontent.com/Sajma27/GoalTracker-AltStore/main/source.json`

The watch package is `build/garmin/GoalTracker.iq`. Upload it to a **new Beta App** in the [Garmin developer dashboard](https://apps-developer.garmin.com/en-US/developer/dashboard), preserving the source manifest UUID and developer signing key for updates. Select a data-field listing and Forerunner 970 support. The beta is installed through the Connect IQ phone app under the uploader's Garmin account. No Garmin listing has been submitted by this repository setup.

For USB testing, copy only `GoalTracker.prg` to `GARMIN/APPS` through an MTP utility on macOS, then add the field to Football's data screens. Do not copy the IPA, IQ package, or developer key into that folder.

Updates: increment both iPhone marketing/build versions as appropriate, regenerate and validate the IPA/source, keep older AltStore version entries, publish a new release, and upload the revised Garmin IQ to its original beta listing. The metadata generator creates a first-release source; for later releases, prepend its version entry to the existing source rather than discarding older versions.
