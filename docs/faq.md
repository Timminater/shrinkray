# FAQ

### Why is CPU usage 10–40% with hardware encoding?

This is normal. The GPU handles video encoding/decoding, but the CPU still handles:
- Demuxing (parsing input container)
- Muxing (writing output container)
- Audio/subtitle stream copying
- FFmpeg process overhead

### Why did Shrinkray skip some files?

Files are automatically skipped if:
- Already encoded in the target codec (HEVC/AV1)
- Already at or below the target resolution (1080p/720p presets)

### What happens if the transcoded file is larger?

Shrinkray rejects files where the output is larger than the original. The original file is kept unchanged.

### Are subtitles preserved?

Yes. All subtitle streams are copied to the output file unchanged (`-c:s copy`).

### Are multiple audio tracks preserved?

Yes. All audio streams are copied to the output file unchanged (`-c:a copy`). If your source has multiple audio tracks (different languages, commentary, etc.), they will all be retained.

### What about HDR content?

Hardware encoders generally preserve HDR metadata and 10-bit color depth. Software encoders may require additional configuration for HDR preservation.
