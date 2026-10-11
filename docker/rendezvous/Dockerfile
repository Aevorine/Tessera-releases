# Tessera cross-network meeting point ("rendezvous").
#
# The binary is a static Go executable taken from the release assets
# (Tessera-Rendezvous-<version>-linux-<arch>.tar.gz), so the image needs no base
# layer at all: nothing to patch, nothing else inside. The workflow in
# .github/workflows/rendezvous-image.yml unpacks the assets into dist/<arch>/.
#
# It listens on UDP. Publish the port with /udp:
#   docker run -d -p 47820:47820/udp ghcr.io/aevorine/tessera-rendezvous -addr :47820 -secret "..."
FROM scratch
ARG TARGETARCH
COPY dist/${TARGETARCH}/tessera-rendezvous /tessera-rendezvous
COPY dist/LICENSE /LICENSE
USER 65532:65532
EXPOSE 47820/udp
ENTRYPOINT ["/tessera-rendezvous"]
CMD ["-addr", ":47820"]
