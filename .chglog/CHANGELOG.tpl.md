{{/* Unreleased version */}}
{{- if .Unreleased -}}
<a name="unreleased"></a>
## [Unreleased]
{{ if .Unreleased.CommitGroups -}}
{{/* NOTE: merge all .CommitGroups using the same `.Title` */}}
{{- $commitGroups := dict -}}
{{- range .Unreleased.CommitGroups -}}
{{- $_ := set $commitGroups (.Title) (concat (get $commitGroups .Title | default list) .Commits) -}}
{{- end -}}
{{ range $title, $commits := $commitGroups -}}
### {{ $title }}
{{ range $commits -}}
- {{ if .Scope }}**{{ .Scope }}:** {{ end }}{{ .Subject }}
{{ end }}
{{ end -}} {{/* end 'range $title, $commits := $commitGroups' */}}
{{- end -}} {{/* end 'if .CommitGroups' */}}
{{- end -}} {{/* end 'if .Unreleased' */}}
{{/* Released versions */}}
{{ range (.Versions | default list) }}
<a name="{{ .Tag.Name }}"></a>
## {{ if .Tag.Previous }}[{{ .Tag.Name }}]{{ else }}{{ .Tag.Name }}{{ end }} - {{ datetime "2006-01-02" .Tag.Date }}
{{ if .CommitGroups -}}
{{/* NOTE: merge all .CommitGroups using the same `.Title` */}}
{{- $commitGroups := dict -}}
{{- range .CommitGroups -}}
{{- $_ := set $commitGroups (.Title) (concat (get $commitGroups .Title | default list) .Commits) -}}
{{- end -}}
{{- range $title, $commits := $commitGroups -}}
### {{ $title }}
{{ range $commits -}}
- {{ if .Scope }}**{{ .Scope }}:** {{ end }}{{ .Subject }}
{{ end }}
{{ end -}} {{/* end 'range $title, $commits := $commitGroups' */}}
{{- end -}} {{/* end 'if .CommitGroups' */}}

{{- if .RevertCommits -}}
### Reverts
{{ range .RevertCommits -}}
- {{ .Revert.Header }}
{{ end }}
{{ end -}} {{/* end 'if .RevertCommits' */}}

{{- if .MergeCommits -}}
### Pull Requests
{{ range .MergeCommits -}}
- {{ .Header }}
{{ end }}
{{ end -}} {{/* end 'if .MergeCommits' */}}

{{- if .NoteGroups -}}
{{ range .NoteGroups -}}
### {{ .Title }}
{{ range .Notes }}
{{ .Body }}
{{ end }}
{{ end -}} {{/* end 'range .NoteGroups' */}}
{{ end -}} {{/* end 'if .NoteGroups' */}}

{{- end -}} {{/* end 'range (.Versions | default list)' */}}

{{- if .Versions }}
[Unreleased]: {{ .Info.RepositoryURL }}/compare/{{ $latest := index .Versions 0 }}{{ $latest.Tag.Name }}...HEAD
{{ range .Versions -}}
{{ if .Tag.Previous -}}
[{{ .Tag.Name }}]: {{ $.Info.RepositoryURL }}/compare/{{ .Tag.Previous.Name }}...{{ .Tag.Name }}
{{ end -}}
{{ end -}}
{{ end -}}