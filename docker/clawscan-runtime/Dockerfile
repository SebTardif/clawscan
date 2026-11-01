FROM python:3.12-slim

ARG SKILLSPECTOR_REF=6d0b7c849949cab9181e57798247f89e48ce5d6a
ARG AIG_SKILL_SCAN_VERSION=0.2.2
ARG CISCO_AI_SKILL_SCANNER_VERSION=2.3.2
ARG SNYK_AGENT_SCAN_VERSION=0.6.10
ARG CLAUDE_CODE_VERSION=2.1.316
ARG OPENAI_CODEX_VERSION=0.166.0
ARG AGENTVERUS_SCANNER_VERSION=0.8.1
ARG SOCKET_CLI_VERSION=1.12.2

ENV PATH="/root/.local/bin:${PATH}" \
    PIP_DISABLE_PIP_VERSION_CHECK=1 \
    PYTHONUNBUFFERED=1

RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl git gnupg \
    && curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && rm -rf /var/lib/apt/lists/*

RUN python -m pip install --no-cache-dir --upgrade pip uv \
    && python -m pip install --no-cache-dir \
      "git+https://github.com/NVIDIA/skillspector.git@${SKILLSPECTOR_REF}" \
      "cisco-ai-skill-scanner==${CISCO_AI_SKILL_SCANNER_VERSION}" \
    && uv tool install "aig-skill-scan==${AIG_SKILL_SCAN_VERSION}" \
    && uv tool install "snyk-agent-scan==${SNYK_AGENT_SCAN_VERSION}" \
    && ln -sf /root/.local/bin/snyk-agent-scan /usr/local/bin/snyk-agent-scan

RUN ln -sf /root/.local/bin/aig-skill-scan /usr/local/bin/aig-skill-scan

RUN npm install -g \
      "@anthropic-ai/claude-code@${CLAUDE_CODE_VERSION}" \
      "@openai/codex@${OPENAI_CODEX_VERSION}" \
      "agentverus-scanner@${AGENTVERUS_SCANNER_VERSION}" \
      "socket@${SOCKET_CLI_VERSION}"

WORKDIR /workspace
