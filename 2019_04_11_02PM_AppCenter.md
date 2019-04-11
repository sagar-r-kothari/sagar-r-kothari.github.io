### Sample Fastlane Script to upload to MS App Center

```py
	desc "Submit a new Build with hockey app with debug mode."
	desc "This will be used for feature dev testing / alpha testing."
	lane :betaHockey do 
	ensure_git_status_clean
	increment_build_number
	build_ios_app(
		scheme: "appTargetScheme",
		configuration: "AdHoc",
		export_method: "ad-hoc"
	)
	appcenter_upload(
		api_token: "API_TOKEN_HERE",
		owner_name: "OWNER_NAME_HERE",
		app_name: "APP_NAME_HERE",
		ipa: "IPA_NAME_HERE"
	)
	commit_version_bump
	add_git_tag
	push_to_git_remote
end
```