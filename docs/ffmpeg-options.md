# FFmpeg Options Reference

Shrinkray automatically selects the best available encoder for your hardware. Below are the exact FFmpeg settings used.

## HEVC Encoders

| Encoder | FFmpeg Encoder | Quality Flag | Default Value | Additional Args |
|---------|----------------|--------------|---------------|-----------------|
| **Software** | `libx265` | `-crf` | 26 | `-preset medium` |
| **VideoToolbox** | `hevc_videotoolbox` | `-b:v` | 35% of source bitrate | `-allow_sw 1` |
| **NVENC** | `hevc_nvenc` | `-cq` | 28 | `-preset p4 -tune hq -rc vbr` |
| **Quick Sync** | `hevc_qsv` | `-global_quality` | 27 | `-preset medium` |
| **VAAPI** | `hevc_vaapi` | `-qp` | 27 | — |

## AV1 Encoders

| Encoder | FFmpeg Encoder | Quality Flag | Default Value | Additional Args |
|---------|----------------|--------------|---------------|-----------------|
| **Software** | `libsvtav1` | `-crf` | 35 | `-preset 6` |
| **VideoToolbox** | `av1_videotoolbox` | `-b:v` | 25% of source bitrate | `-allow_sw 1` |
| **NVENC** | `av1_nvenc` | `-cq` | 32 | `-preset p4 -tune hq -rc vbr` |
| **Quick Sync** | `av1_qsv` | `-global_quality` | 32 | `-preset medium` |
| **VAAPI** | `av1_vaapi` | `-qp` | 32 | — |

> **Note:** VideoToolbox uses dynamic bitrate calculation instead of CRF. Target bitrate is calculated as a percentage of source bitrate, clamped between 500 kbps and 15,000 kbps.

## Hardware Decoding

When hardware encoding is active, Shrinkray enables hardware decoding for a full GPU pipeline:

| Platform | Input Args |
|----------|------------|
| **VideoToolbox** | `-hwaccel videotoolbox` |
| **NVENC** | `-hwaccel cuda -hwaccel_output_format cuda` |
| **Quick Sync** | `-hwaccel qsv -hwaccel_output_format qsv` |
| **VAAPI** | `-vaapi_device /dev/dri/renderD128 -hwaccel vaapi -hwaccel_output_format vaapi` |

## Scaling Filters (1080p/720p Presets)

| Platform | Scale Filter |
|----------|--------------|
| **Software** | `scale=-2:'min(ih,1080)'` |
| **NVENC** | `scale_cuda=-2:'min(ih,1080)'` |
| **Quick Sync** | `scale_qsv=-2:'min(ih,1080)'` |
| **VAAPI** | `scale_vaapi=w=-2:h='min(ih,1080)'` |
| **VideoToolbox** | `scale=-2:'min(ih,1080)'` (CPU) |
